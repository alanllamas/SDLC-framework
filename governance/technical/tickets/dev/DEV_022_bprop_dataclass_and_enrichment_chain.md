# DEV TICKET: DEV_022 — BPROP Dataclass & Enrichment Chain
**version_tag:** v0.7.0 — Innovation Proposal & Project Registry
**Parent TREQ:** TREQ_023
**Trace:** @trace TREQ_023

## 1. Development Process Boundaries
* Implement BPropPayload and EnrichmentEntry Pydantic models per TREQ_023
* Implement Librarian.write_bprop() — sequential BPROP_ID,
  atomic write to governance/bprops/pending/, updates
  state.yaml.session.pending_bprops
* Implement Librarian.enrich_bprop() — in-place atomic rewrite
  appending enrichment entry
* Implement upstream order derivation from originating_layer
  per BDR_001 layer hierarchy

## 2. Acceptance Criteria
* BPROP_ID sequential, no collisions
* enrich_bprop() rewrites same file path, enrichment_chain
  grows by one entry per call
* Enrichment order matches layers strictly above originating_layer,
  ascending
* state.yaml.session.pending_bprops updated atomically on write

## 3. Pre-Defined Testing Assignment
See TEST_022

## 4. Documentation Assignment
User manual placeholder: "Proposing Improvements to the Framework"
