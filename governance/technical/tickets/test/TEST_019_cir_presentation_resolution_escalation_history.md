# TEST TICKET: TEST_019 — CIR Presentation, Resolution, Escalation & History
**Parent DEV:** DEV_019 | **Parent TREQ:** TREQ_020

## 1. Test Goal
Verify CIR presentation format, resolution, escalation, and
history query work correctly, and v0.5.0 retrofit succeeds.

## 2. Test Environment
Mock Librarian, temporary pending/resolved CIR directories.

## 3. Implementation Plan
* Unit: rendered CIR contains THE CONFLICT / AGENT PROPOSALS /
  YOUR DECISION sections exactly
* Unit: agent proposals labeled with originating agent, never
  presented as decisions
* Unit: resolve_cir() moves pending→resolved via archive_asset,
  updates state.yaml, generates Ledger entry, unfreezes branch
* Unit: escalation updates loop LOOP_3→LOOP_2→LOOP_1, logs
  CIR_ESCALATED
* Unit: query_cir_history() returns scannable summary without
  full detail
* Integration: TREQ_018 constraint conflict scenario now
  produces a CIR with LOOP_2, not a TQUERY
