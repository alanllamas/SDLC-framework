# TEST TICKET: TEST_020 — Decision History Query
**Parent DEV:** DEV_020 | **Parent TREQ:** TREQ_021

## 1. Test Goal
Verify history queries return correct, properly ordered results
and trace parent_id chains correctly.

## 2. Test Environment
Temporary Global History Ledger with sample entries spanning
v0.1.0-v0.5.0 events (DOCUMENT_AUTHORIZED, TQUERY_RESOLVED, etc).

## 3. Implementation Plan
* Unit: query_history() returns entries reverse-chronologically
* Unit: filtering by artifact_id includes both subject and
  superseded-document matches
* Unit: "why was this decided" tracer follows parent_id chain
  to correct governing BDR/BREQ
* Unit: timeline renders one line per entry by default
* Unit: expand option shows full entry detail
