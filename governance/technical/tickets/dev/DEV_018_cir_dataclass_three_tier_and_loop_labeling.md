# DEV TICKET: DEV_018 — CIR Dataclass, Three-Tier Construction & Loop Labeling
**version_tag:** v0.6.0 — Conflict Intelligence & History
**Parent TREQ:** TREQ_019
**Trace:** @trace TREQ_019

## 1. Development Process Boundaries
* Implement CIRPayload and AgentProposal Pydantic models per TREQ_019
* Implement loop derivation from layers_involved
* Implement Librarian.write_cir() — sequential CIR_ID, atomic
  write to governance/bprops/cirs/pending/, updates
  state.yaml.session.pending_cirs, freezes active_branch

## 2. Acceptance Criteria
* Loop correctly derived per layers_involved mapping
* CIR_ID sequential, no collisions
* state.yaml.session.pending_cirs updated atomically with write
* active_branch frozen until CIR resolved

## 3. Pre-Defined Testing Assignment
See TEST_018

## 4. Documentation Assignment
User manual placeholder: "Understanding Conflict Reports"
