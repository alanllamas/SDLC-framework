# TEST TICKET: TEST_025 — Active Project Switching & State Isolation
**Parent DEV:** DEV_025 | **Parent TREQ:** TREQ_026

## 1. Test Goal
Verify flush-before-load sequence and strict state isolation
between projects.

## 2. Test Environment
Two mock projects (PRJ_001, PRJ_002) with distinct state.yaml
and context.md fixtures.

## 3. Implementation Plan
* Unit: switching PRJ_001 → PRJ_002 flushes PRJ_001 state before
  loading PRJ_002
* Unit: in-memory state after switch contains zero fields from PRJ_001
* Unit: switch to ARCHIVED project rejected, PRJ_001 state already
  flushed and intact
* Unit: switch to non-existent project_id rejected with clear error
* Unit: simulated flush failure aborts switch — operator remains
  on PRJ_001 with original state
* Unit: both projects' last_active timestamps updated post-switch
