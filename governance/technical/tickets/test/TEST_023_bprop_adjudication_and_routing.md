# TEST TICKET: TEST_023 — BPROP Adjudication & Routing
**Parent DEV:** DEV_023 | **Parent TREQ:** TREQ_024

## 1. Test Goal
Verify presentation, adjudication capture, and routing for all
three outcomes (ACCEPT/DEFER/REJECT).

## 2. Test Environment
Mock Librarian with a fully-enriched BPROP fixture
(3 enrichment entries).

## 3. Implementation Plan
* Unit: presentation shows all 3 enrichment entries in collection order
* Unit: ACCEPT records resulting_artifact link, does not call
  document generation itself
* Unit: DEFER appends entry to Future Horizon Ledger
  (01_business_request.md)
* Unit: REJECT records rationale, no Future Horizon entry,
  no resulting_artifact
* Unit: all three outcomes move pending→resolved via archive_asset
* Unit: all three outcomes clear pending_bprops and generate
  Ledger entry with correct event (BPROP_ACCEPT/DEFER/REJECT)
