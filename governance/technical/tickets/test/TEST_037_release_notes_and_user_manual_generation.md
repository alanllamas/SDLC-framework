# TEST TICKET: TEST_037 — Release Notes & User Manual Delta Generation
**Parent DEV:** DEV_037 | **Parent TREQ:** TREQ_038

## 1. Test Goal
Verify grouping, generation, diff/append behavior, and consumed
flag updates.

## 2. Test Environment
Sample CHRONICLE entries spanning multiple milestones and ticket
types (FEATURE/BUG/DEBT), sample DEV_TICKETs with
documentation_assignment fields.

## 3. Implementation Plan
* Unit: chronicles grouped correctly by version_tag.milestone
* Unit: release notes include Features/Fixes/Internal
  Improvements sections only when corresponding types present
* Unit: existing RELEASE_NOTES file → diff/append via TDR_008,
  not overwritten
* Unit: user manual slug correctly derived from
  documentation_assignment text
* Unit: existing user manual section file → content merged
  under existing heading
* Integration: sign-off sets consumed_by_agent_8=true on all
  contributing chronicles and logs CHRONICLE_CONSUMED per chronicle
