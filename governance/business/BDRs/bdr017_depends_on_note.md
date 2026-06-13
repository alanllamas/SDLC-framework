# Cross-Reference Note — BDR_017 depends_on

FLAGGED FOR GIR_001 VERIFICATION (intentionally left as-is per
HITL instruction 2026-06-03):

BDR_017 (Non-Destructive Operations Mandate) declares
`depends_on: [BDR_003, BDR_008, BDR_010]`. BDR_010 was
superseded by BDR_015 (Repository Structure & Core Governance).
The depends_on reference is stale — points to an archived
document rather than its AUTHORIZED successor.

This is left intentionally unresolved as a test case for
GIR_001's Plane 1 (ascending traceability / supersession-chain
resolution) — confirm Plane 1 either:
  (a) flags this as a finding (depends_on should resolve through
      supersession chains to the current AUTHORIZED artifact), or
  (b) explicitly treats depends_on references to superseded-but-
      archived documents as acceptable (historical reference).

If GIR_001 does NOT catch this, Plane 1 (TREQ_029) needs a
follow-up refinement. If GIR_001 DOES catch it, resolve by
updating BDR_017's depends_on to BDR_015.
