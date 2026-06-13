# DEV TICKET: DEV_029 — GIR Report Generation & Phase 1 Close
**version_tag:** v0.8.0 — MVP
**Parent TREQ:** TREQ_030
**Trace:** @trace TREQ_030

## 1. Development Process Boundaries
* Implement generate_gir() per TREQ_030 — What/Plane/Why/When/
  Resolution-criteria format
* Implement GIR supersession chain (GIR_001 → GIR_002...) per RULE_001
* Implement per-item Yellow justification capture flow —
  no batch approval without text
* Implement attempt_phase_1_close() — red/yellow gating,
  GIR sign-off via TDR_008, state.yaml integrity_audit and
  phase_gates.phase_1_closed updates, PHASE_1_CLOSED Ledger event

## 2. Acceptance Criteria
* GIR document follows required format per finding
* Second GIR references first via supersession — sequential IDs only
* Unjustified Yellow item blocks phase 1 close
* Any Red item blocks phase 1 close entirely
* Successful close: GIR AUTHORIZED, state.yaml updated,
  PHASE_1_CLOSED logged

## 3. Pre-Defined Testing Assignment
See TEST_029

## 4. Documentation Assignment
User manual placeholder: "Closing Phase 1 — The Integrity Audit"
