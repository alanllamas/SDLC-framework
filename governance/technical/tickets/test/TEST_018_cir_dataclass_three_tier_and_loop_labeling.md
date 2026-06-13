# TEST TICKET: TEST_018 — CIR Dataclass, Three-Tier Construction & Loop Labeling
**Parent DEV:** DEV_018 | **Parent TREQ:** TREQ_019

## 1. Test Goal
Verify CIR payload construction, loop derivation, and pipeline
integration with state.yaml.

## 2. Test Environment
Temporary governance directory, mock Librarian.

## 3. Implementation Plan
* Unit: layers_involved ["Business","Product"] → LOOP_1
* Unit: layers_involved ["Product","Architecture"] → LOOP_2
* Unit: layers_involved ["Architecture","Implementation"] → LOOP_3
* Unit: CIR_ID sequential across pending/ and resolved/
* Unit: state.yaml.session.pending_cirs updated atomically with write
* Integration: active_branch frozen — further agent actions
  blocked until CIR resolved
