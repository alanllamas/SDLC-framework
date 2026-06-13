---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_015
type: TREQ
title: "Document Rendering & Diff Presentation"
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

# Technical Requirement: TREQ_015 — Document Rendering & Diff Presentation

## 1. Intent & Scope
Defines how the Orchestrator renders a document for operator
review, including diff generation for existing DRAFTs.

## 2. Technical Constraints

### Rendering Logic
```python
def render_document_for_review(
    new_content: str,
    existing_path: str | None,
    librarian: "Librarian"
) -> str:
    output = []
    if existing_path:
        prev_result = librarian.read_file(existing_path)
        if prev_result.success:
            diff = generate_unified_diff(
                prev_result.data, new_content,
                fromfile="current (DRAFT)",
                tofile="proposed"
            )
            output.append(f"### Changes\n```diff\n{diff}\n```")
    output.append(f"### Full Document\n```markdown\n{new_content}\n```")
    return "\n\n".join(output)
```
* `generate_unified_diff` uses Python's `difflib.unified_diff`.
* If `new_content` exceeds TDR_003 active artifact budget
  (2,000 tokens), split into chunks with header
  `[Part N of M]` — diff section never split, only full content.
* If no `existing_path` (new document), diff section omitted entirely.

## 3. Verification & QA Criteria
* New document: only full content shown, no diff section.
* Modified DRAFT: diff shown above full content.
* Document exceeding token budget: chunked with `[Part N of M]` headers.
* Diff correctly reflects line-level changes via difflib.
