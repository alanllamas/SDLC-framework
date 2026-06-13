# TEST TICKET: TEST_013 — TQUERY Presentation & Resolution Flow
**Parent DEV:** DEV_013 | **Parent TREQ:** TREQ_014

## 1. Test Goal
Verify TQUERY presentation format, resolution capture, and
non-destructive archival.

## 2. Test Environment
MockLLMAdapter, temporary governance directory, mock Librarian.

## 3. Implementation Plan
* Unit: presentation template renders correctly with all fields
* Unit: long problem_statement (>200 words) truncated with file pointer
* Unit: option C captures custom free-text input
* Unit: resolve_tquery moves file pending/ → resolved/ via archive_asset
* Unit: pending/ file absent, resolved/ file present with hitl_decision
* Unit: state.yaml.session.pending_tqueries no longer contains resolved ID
* Unit: Ledger entry generated on resolution
* Integration: full cycle — agent constructs TQUERY, operator resolves,
  branch unfreezes
