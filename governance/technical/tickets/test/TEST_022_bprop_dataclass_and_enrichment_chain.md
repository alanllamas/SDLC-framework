# TEST TICKET: TEST_022 — BPROP Dataclass & Enrichment Chain
**Parent DEV:** DEV_022 | **Parent TREQ:** TREQ_023

## 1. Test Goal
Verify BPROP construction, sequential ID assignment, and
in-place enrichment chain growth.

## 2. Test Environment
Temporary governance/bprops/ directory, mock Librarian.

## 3. Implementation Plan
* Unit: BPROP_ID sequential across pending/ and resolved/
* Unit: enrich_bprop() rewrites same file, enrichment_chain
  grows by exactly one entry
* Unit: BPROP originating at Technical collects Architecture
  then Product entries, in that order
* Unit: BPROP originating at Architecture skips Technical,
  collects only Product
* Unit: state.yaml.session.pending_bprops updated atomically on write
