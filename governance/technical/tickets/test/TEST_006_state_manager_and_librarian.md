# TEST TICKET: TEST_006 — State Manager & Librarian
**Parent DEV:** DEV_006 | **Parent TREQ:** TREQ_007, TREQ_008

## 1. Test Goal
Verify state schema validation rejects invalid states and
atomic writes protect against corruption.

## 2. Test Environment
Temporary state files. Pydantic models as validation engine.

## 3. Implementation Plan
* Unit: valid state.yaml passes validation
* Unit: invalid state.yaml (missing required field) rejected before write
* Unit: schema version mismatch detected and surfaced
* Unit: StateManager.save() uses atomic pattern
* Integration: Librarian.write() chain validates then writes atomically
