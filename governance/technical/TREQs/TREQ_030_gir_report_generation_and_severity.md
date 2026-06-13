---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_030
type: TREQ
title: "GIR Report Generation & Severity Classification"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_015
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

# Technical Requirement: TREQ_030 — GIR Report Generation & Severity Classification

## 1. Intent & Scope
Defines how Five-Plane Verification findings (TREQ_029) are
aggregated into a GIR document, how Yellow items require HITL
deferral justification, and how Phase 1 closure is gated.

## 2. Technical Constraints

### GIR Document Generation
```python
def generate_gir(findings, librarian):
    gir_id = librarian.next_sequential_id("GIR")  # GIR_001, GIR_002...
    red = [f for f in findings if f.severity == "RED"]
    yellow = [f for f in findings if f.severity == "YELLOW"]

    sections = [f"# {gir_id} — Governance Integrity Report"]
    sections.append(f"Red items: {len(red)} | Yellow items: {len(yellow)}")

    for f in red + yellow:
        justification = ""
        if f.severity == "YELLOW":
            justification = "Inherited from ingestion check: [pending HITL input]"
        sections.append(
            f"### [{f.severity}] {f.artifact_id} - Plane {f.plane}\n"
            f"What: {f.what}\n"
            f"Plane: {f.plane}\n"
            f"Why: {f.why}\n"
            f"When: [pending HITL input if YELLOW]\n"
            f"Resolution criteria: {f.resolution_criteria}\n"
            f"{justification}"
        )
    return "\n\n".join(sections)
```

* Supersession chain per RULE_001: if a prior GIR exists,
  new GIR's `history.supersedes_document` references it —
  sequential IDs, never `_v2` suffixes.

### Yellow Item Deferral Justification
* Every Yellow item's "Inherited from ingestion check" field
  MUST be filled by HITL before sign-off — Orchestrator presents
  each Yellow item individually for this input, cannot be
  batch-approved without text.
* If HITL declines to justify a Yellow item, Orchestrator offers:
  resolve now (route to appropriate agent) or escalate to CIR.

### Phase 1 Closure Gate
```python
def attempt_phase_1_close(gir_doc, red_count, yellow_justifications, hitl_identity, librarian):
    if red_count > 0:
        return AdapterResult(success=False,
            error=f"{red_count} Red items must be resolved before Phase 1 can close")
    if any(not j for j in yellow_justifications.values()):
        return AdapterResult(success=False,
            error="All Yellow items require deferral justification")

    # Sign off GIR via TDR_008 sign-off flow (status DRAFT -> AUTHORIZED)
    librarian.sign_off_document(gir_doc, hitl_identity)

    # Update state
    state = librarian.load_current_state()
    state.integrity_audit.status = "PASSED"
    state.integrity_audit.red_items_count = 0
    state.integrity_audit.yellow_items_count = len(yellow_justifications)
    state.phase_gates.phase_1_closed = True
    librarian.save_state(state)

    librarian.log_ledger_entry(
        event="PHASE_1_CLOSED",
        artifact_id=gir_doc_id(gir_doc),
        signatory=hitl_identity
    )
    return AdapterResult(success=True)
```

## 3. Verification & QA Criteria
* GIR document follows What/Plane/Why/When/Resolution-criteria
  format per item.
* Second GIR in a project references first via supersession chain
  (GIR_002 supersedes GIR_001) — sequential IDs only.
* Yellow item without filled justification blocks
  attempt_phase_1_close().
* Any Red item present blocks attempt_phase_1_close() entirely.
* Successful close: GIR signed off (AUTHORIZED), state.yaml
  integrity_audit and phase_gates.phase_1_closed updated,
  PHASE_1_CLOSED Ledger event generated.
