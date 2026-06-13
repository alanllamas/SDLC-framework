---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_043
type: TREQ
title: "Plane 4 — Target-Version-Aware Pending Resolution Check"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_015
  horizontal:
    depends_on: [TREQ_029]
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

# Technical Requirement: TREQ_043 — Plane 4 Target-Version-Aware Pending Resolution Check

## 1. Intent & Scope
Refines Plane 4 (`plane_4_pending_resolution()`, TREQ_029) so
that pending TQUERY/CIR/BPROP items explicitly scoped to a
**future** version do not block the current version's Phase 1
closure. This is **additive** to TREQ_029 — the original
function signature and Red-by-default behavior are unchanged
for items with no `target_version` or a `target_version`
matching the version under audit.

## 2. Technical Constraints

### 2.1 TQUERY Schema Extension
`governance/templates/TQUERY_metadata.yaml` gains an optional
field:

```yaml
tquery:
  # ... existing fields ...
  target_version: null  # optional, e.g. "v2.0.0". If null,
                         # defaults to the project's current
                         # in-progress version.
```

This is an additive schema change to a template — no
supersession required (templates are not AUTHORIZED governance
decisions, they are reusable scaffolding per BDR_016).

### 2.2 Refined Plane 4
```python
def plane_4_pending_resolution(
    librarian: "Librarian", audit_version: str
) -> list["Finding"]:
    findings = []
    for pending_item in librarian.scan_pending_bprops():
        item_version = pending_item.get("target_version")
        if item_version is None or item_version == audit_version:
            findings.append(Finding(
                artifact_id=pending_item["id"], plane=4, severity="RED",
                what=f"{pending_item['id']} pending resolution",
                why="TQUERY/CIR/BPROP must be resolved before "
                    f"{audit_version} Phase 1 can close",
                resolution_criteria="Resolve via assigned resolution_owner"
            ))
        else:
            findings.append(Finding(
                artifact_id=pending_item["id"], plane=4, severity="GREEN",
                what=f"{pending_item['id']} pending, scoped to {item_version}",
                why=f"Correctly deferred — does not block {audit_version}",
                resolution_criteria=f"Will be evaluated during {item_version} audit"
            ))
    return findings
```

* `audit_version` is passed by `run_five_plane_audit()` —
  sourced from `state.yaml.session.current_version` (the
  version currently being planned/audited).
* GREEN findings for correctly-deferred items still appear in
  the GIR report's full findings list (for visibility) but do
  NOT count toward `red_count` in `attempt_phase_1_close()`
  (TREQ_030) — only RED/YELLOW counts are gating.

## 3. Verification & QA Criteria
* Pending item with no `target_version` → RED (unchanged
  default behavior from TREQ_029).
* Pending item with `target_version` == `audit_version` → RED.
* Pending item with `target_version` != `audit_version`
  (e.g., "v2.0.0" while auditing "v1.0.0") → GREEN, appears
  in report as correctly-deferred, does not block closure.
* `attempt_phase_1_close()` counts only RED/YELLOW from Plane 4
  — GREEN deferred items do not increment `red_count`.
* TQUERY_metadata.yaml template accepts optional
  `target_version` field without breaking existing TQUERYs
  that omit it (defaults to null → current-version behavior).
