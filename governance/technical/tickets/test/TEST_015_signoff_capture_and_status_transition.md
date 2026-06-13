# TEST TICKET: TEST_015 — Sign-off Capture & Status Transition
**Parent DEV:** DEV_015 | **Parent TREQ:** TREQ_016

## 1. Test Goal
Verify sign-off transitions are atomic, validated, and
non-destructive for supersessions.

## 2. Test Environment
Temporary governance directory with mock DRAFT and AUTHORIZED documents.

## 3. Implementation Plan
* Unit: sign-off on REVIEW or AUTHORIZED status rejected
* Unit: sign-off on DRAFT updates status to AUTHORIZED + hitl_signatory
* Unit: supersession referencing non-existent document rejected
  before any write
* Unit: supersession referencing non-AUTHORIZED document rejected
* Unit: successful supersession moves previous doc to rollbacks
  via archive_asset — file still exists at new path
* Unit: Ledger entry generated with correct event type and signatory
* Integration: simulated failure mid-transaction leaves no partial
  state (front-matter unchanged if write fails)
