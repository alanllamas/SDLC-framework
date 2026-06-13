---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_028
type: TREQ
title: "Layer Seal & Phase Transition State Updates"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_014
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

# Technical Requirement: TREQ_028 — Layer Seal & Phase Transition State Updates

## 1. Intent & Scope
Defines the atomic operation executed on HITL confirmation of
an ICD, sealing the layer and unlocking the next layer's agent
per ADR_004 section 3.3.

## 2. Technical Constraints

```python
def seal_layer(
    layer: str,
    icd_result: "ICDResult",
    hitl_identity: str,
    librarian: "Librarian",
    orchestrator: "Orchestrator"
) -> "AdapterResult":
    if icd_result.overall_status == "FAILED":
        return AdapterResult(success=False,
            error="Cannot seal layer with FAILED ICD")

    # 1. Append ICD section to 01_[layer]_baseline.md
    librarian.append_icd_to_baseline(layer, icd_result, hitl_identity)

    # 2. Update state.yaml
    state = orchestrator.current_state
    state.phase_gates[f"{layer}_layer_sealed"] = True
    state.ingestion_compliance[
        f"{layer}_to_{next_layer(layer)}"
    ] = icd_result.overall_status
    librarian.save_state(state)  # atomic per TREQ_007

    # 3. Ledger entry
    librarian.log_ledger_entry(
        event="LAYER_SEALED",
        artifact_id=f"{layer}_layer",
        signatory=hitl_identity,
        details=f"ICD: {icd_result.overall_status}"
    )

    # 4. Trigger role transition to next layer's agent (TREQ_012)
    if next_layer(layer) != "DONE":
        orchestrator.propose_transition(
            to_agent=agent_for_layer(next_layer(layer)),
            reason=f"{layer} layer sealed — {icd_result.overall_status}"
        )
    else:
        # All four layers sealed — ready for GIR per TREQ_029/030
        librarian.log_ledger_entry(
            event="ALL_LAYERS_SEALED",
            artifact_id="phase_1",
            signatory=hitl_identity
        )

    return AdapterResult(success=True, data=layer)
```

* Steps 1-3 execute as a single transaction — if step 2 (state
  write) fails, step 1's baseline append must be rolled back
  (re-read baseline, remove appended section) before returning
  failure. Both are file writes via Librarian — sequencing
  ensures step 1 only persists if step 2 succeeds, by performing
  step 2's validation before step 1's write.
* Step 4 reuses TREQ_012's two-step transition — this still
  presents the cooling-off Transition Summary to the operator,
  it is not silent.

## 3. Verification & QA Criteria
* FAILED ICD rejected — no state or baseline changes.
* Successful seal: baseline updated, phase_gates and
  ingestion_compliance updated atomically, Ledger entry generated.
* Partial failure (state write fails after baseline append)
  results in no persisted baseline change — verified via
  validate-before-write ordering.
* Sealing Technical layer (last) generates ALL_LAYERS_SEALED
  event instead of a transition proposal.
* Role transition still presents Transition Summary — TREQ_012
  cooling-off gate not bypassed.
