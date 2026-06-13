---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_016
type: TREQ
title: "Sign-off Capture & Atomic Status Transition"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.4.0 — Document Generation & Sign-off"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_008
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_016 — Sign-off Capture & Atomic Status Transition

## 1. Intent & Scope
Defines the atomic operation that transitions a document's
status upon HITL sign-off, including supersession validation,
per ADR_004 section 3.3 and RULE_005.

## 2. Technical Constraints

### Sign-off Operation
```python
def sign_off_document(
    path: str,
    content: str,
    front_matter: dict,
    hitl_identity: str,
    librarian: "Librarian"
) -> "AdapterResult":
    # 1. RULE_005 check — only DRAFT → AUTHORIZED transitions allowed here
    if front_matter["status"] != "DRAFT":
        return AdapterResult(success=False,
            error="Only DRAFT documents can be signed off")

    # 2. Supersession validation (if applicable)
    supersedes = front_matter.get("history", {}).get("supersedes_document", {}).get("document_id")
    if supersedes:
        prev = librarian.read_file(resolve_path(supersedes))
        if not prev.success or prev_status(prev.data) != "AUTHORIZED":
            return AdapterResult(success=False,
                error=f"Cannot supersede {supersedes} — not found or not AUTHORIZED")

    # 3. Update front-matter
    front_matter["status"] = "AUTHORIZED"
    front_matter["provenance_ledger"]["hitl_signatory"] = hitl_identity

    # 4. Atomic write (content + front-matter as single file)
    final_content = render_with_frontmatter(front_matter, content)
    write_result = librarian.write_file(path, final_content)
    if not write_result.success:
        return write_result

    # 5. If supersession — archive previous AUTHORIZED doc
    if supersedes:
        librarian.archive_asset(
            resolve_path(supersedes),
            f"governance/bprops/rollbacks/{supersedes}_superseded.md"
        )

    # 6. Ledger entry
    librarian.log_ledger_entry(
        event="DOCUMENT_AUTHORIZED",
        artifact_id=front_matter["id"],
        signatory=hitl_identity,
        supersedes=supersedes
    )

    return AdapterResult(success=True, data=path)
```

* All steps 3-6 execute as a single Librarian transaction —
  if any step fails, no partial state persists (validate before
  any write occurs, per steps 1-2 happening before step 4).
* Supersession archival uses `archive_asset()` — non-destructive
  per BDR_020, never `secure_delete()`.

## 3. Verification & QA Criteria
* Attempting sign-off on non-DRAFT document rejected with clear error.
* Supersession referencing non-existent or non-AUTHORIZED document rejected.
* Successful sign-off: front-matter updated, file written, Ledger entry created.
* Successful supersession: previous document moved to rollbacks via archive_asset.
* No partial state possible — validation occurs before any write.
