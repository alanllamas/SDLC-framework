---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_041
type: TREQ
title: "Mandatory Chronicle Coverage at Technical Layer Closeout"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_014
  horizontal:
    depends_on: [TREQ_027, TDR_018, TDR_019]
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

# Technical Requirement: TREQ_041 — Mandatory Chronicle Coverage at Technical Layer Closeout

## 1. Intent & Scope
Adds a Technical-layer-specific additional check to
`generate_icd()` (TREQ_027) — every DEV_TICKET marked `DONE`
during the closeout cycle must have a corresponding `CHRONICLE`
in `status: AUTHORIZED`. This is **additive** to TREQ_027 —
TREQ_027 itself is unchanged; this check is invoked only when
`layer == "Technical"`, giving Agent 9's per-ticket chronicle
check (TREQ_040, YELLOW/advisory) a batch-level enforcement
counterpart (RED/blocking).

## 2. Technical Constraints

```python
def check_chronicle_coverage(
    layer: str, librarian: "Librarian"
) -> list["Finding"]:
    if layer != "Technical":
        return []

    findings = []
    done_tickets = librarian.scan_dev_tickets_by_status("DONE", since="last_closeout")
    for ticket in done_tickets:
        chronicle_id = f"CHRONICLE_{ticket['id']}"
        chronicle = librarian.read_chronicle_if_exists(chronicle_id)
        if not chronicle or chronicle["status"] != "AUTHORIZED":
            findings.append(Finding(
                artifact_id=ticket["id"], plane=2, severity="RED",
                what=f"No AUTHORIZED chronicle for completed ticket {ticket['id']}",
                why="Mandatory chronicle coverage per TREQ_041 — "
                    "completed work must be chronicled before layer seal",
                resolution_criteria="Run Technical Process Chronicler "
                    "(Agent 7) for this ticket and sign off the chronicle"
            ))
    return findings

# generate_icd() (TREQ_027) calls this and merges results:
#   icd_result.conflicts += [f.what for f in check_chronicle_coverage(layer, librarian)]
#   if findings: overall_status = "FAILED"
```

* "since last_closeout" — tickets completed since the previous
  `LAYER_SEALED` event for Technical (or project start if none).
* This check runs ONLY at Technical layer closeout — Business/
  Product/Architecture closeouts unaffected.
* A `DONE` ticket with chronicle still in `DRAFT` (Agent 7 ran
  but not signed off) counts as a finding — operator must
  complete the sign-off, not just generate the draft.

## 3. Verification & QA Criteria
* Business/Product/Architecture closeouts: check_chronicle_coverage()
  returns empty list unconditionally.
* Technical closeout, all DONE tickets have AUTHORIZED chronicles:
  empty list, generate_icd() unaffected.
* Technical closeout, one DONE ticket missing chronicle entirely:
  RED finding, overall_status FAILED.
* Technical closeout, one DONE ticket with DRAFT (unsigned)
  chronicle: RED finding, overall_status FAILED.
* Resolution: operator runs Agent 7 + sign-off for flagged
  ticket, re-running closeout produces empty findings.
