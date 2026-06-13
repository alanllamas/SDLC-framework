# TEST TICKET: TEST_012 — TQUERY Dataclass & Validation
**Parent DEV:** DEV_012 | **Parent TREQ:** TREQ_013

## 1. Test Goal
Verify TQUERY payload validation and ID assignment work correctly.

## 2. Test Environment
Temporary governance directory structure with mock pending/resolved files.

## 3. Implementation Plan
* Unit: missing required field rejected before write
* Unit: TQ_ID assignment scans both pending/ and resolved/ for max ID
* Unit: written YAML matches TQUERY_metadata.yaml schema
* Unit: state.yaml.session.pending_tqueries updated atomically with write
* Integration: simulated crash mid-write leaves no orphaned TQUERY file
