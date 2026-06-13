# TEST TICKET: TEST_017 — Vision Ledger, Active Constraints & Conflict Scan
**Parent DEV:** DEV_017 | **Parent TREQ:** TREQ_018

## 1. Test Goal
Verify routing, anti-goal scanning, and conflict detection
operate correctly and atomically.

## 2. Test Environment
Temporary 01_business_request.md with sample Anti-Goals,
Active Constraints, and Vision Ledger sections.

## 3. Implementation Plan
* Unit: choice A with technology matching an Anti-Goal —
  blocked, AdapterResult contains ANTI_GOAL_VIOLATION + anti-goal text
* Unit: choice A with technology conflicting existing constraint —
  AdapterResult contains CONSTRAINT_CONFLICT + conflict details
* Unit: choice A with clean technology — appended to Active
  Constraints table atomically, Ledger entry generated
* Unit: choice B — appended to Vision Ledger with PREFERENCE
  tag atomically
* Unit: matches_anti_goal() and detect_conflict() execute with
  zero LLM calls (verified via mock call counter)
* Integration: ANTI_GOAL_VIOLATION path presents 2 compliant
  options to operator per PREQ_009 section 2.4
