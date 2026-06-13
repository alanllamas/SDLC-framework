# DEV TICKET: DEV_023 — BPROP Adjudication & Routing
**version_tag:** v0.7.0 — Innovation Proposal & Project Registry
**Parent TREQ:** TREQ_024
**Trace:** @trace TREQ_024

## 1. Development Process Boundaries
* Implement BPROP presentation template per TREQ_024 showing
  full enrichment_chain in collection order
* Implement Adjudication model and adjudicate_bprop() per TREQ_024
* Implement Librarian.update_bprop_adjudication()
* Implement Librarian.append_future_horizon_entry() for DEFER
* Implement archive_asset() move + pending_bprops cleanup +
  Ledger entry for all three outcomes

## 2. Acceptance Criteria
* Presentation shows all enrichment entries in order collected
* ACCEPT records resulting_artifact link only — generation via
  TDR_008 sign-off flow separately
* DEFER appends to Future Horizon Ledger
* REJECT records rationale only
* All outcomes: pending→resolved move, pending_bprops cleared,
  Ledger entry generated

## 3. Pre-Defined Testing Assignment
See TEST_023

## 4. Documentation Assignment
User manual placeholder: "How Proposals Get Decided"
