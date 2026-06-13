# TEST TICKET: TEST_029 — GIR Report Generation & Phase 1 Close
**Parent DEV:** DEV_029 | **Parent TREQ:** TREQ_030

## 1. Test Goal
Verify GIR document structure, supersession chain, Yellow
justification enforcement, and Phase 1 close gating.

## 2. Test Environment
Sample Finding lists (zero red, some red, some yellow),
temporary governance/audits/ directory.

## 3. Implementation Plan
* Unit: generate_gir() output contains What/Plane/Why/When/
  Resolution criteria per finding
* Unit: second generate_gir() call produces GIR_002 with
  supersedes_document referencing GIR_001
* Unit: attempt_phase_1_close() with red_count > 0 → rejected,
  no state change
* Unit: attempt_phase_1_close() with unfilled yellow
  justification → rejected
* Unit: attempt_phase_1_close() with red_count == 0 and all
  yellows justified → GIR signed off AUTHORIZED, state.yaml
  phase_gates.phase_1_closed == true, PHASE_1_CLOSED logged
* Integration: full audit → GIR → justification → close cycle
  using real v0.1.0-v0.7.0 artifact set
