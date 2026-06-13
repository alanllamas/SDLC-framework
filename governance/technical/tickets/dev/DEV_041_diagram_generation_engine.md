# DEV TICKET: DEV_041 — Diagram Generation Engine
**version_tag:** v0.12.0 — Agent 9: Compliance Gatekeeper
**Parent TREQ:** TREQ_042
**Trace:** @trace TREQ_042

## 1. Development Process Boundaries
* Implement generate_diagram() per TREQ_042 — full/layer/artifact scopes
* Implement filter_by_layer(), filter_edges_to_nodes(),
  all_edges() helpers over TREQ_022's graph
* Implement derive_loop_edges() — queries query_cir_history()
  (TREQ_020), maps related_artifacts pairs to loop value
* Implement Mermaid graph TD rendering with dashed loop edges
* Implement /diagram [scope] Orchestrator command, available
  in any phase
* Atomic write to governance/diagrams/[scope_slug]_[timestamp].mmd,
  no front-matter status field

## 2. Acceptance Criteria
* scope="full" includes every graph artifact
* scope="layer:Technical" includes Technical types + one-hop
  cross-layer context
* scope="artifact:[ID]" matches impact_map() direct+indirect exactly
* Resolved CIR with loop="LOOP_2" renders as dashed labeled edge
* Output is valid Mermaid graph TD syntax
* /diagram available regardless of phase_gates state

## 3. Pre-Defined Testing Assignment
See TEST_041

## 4. Documentation Assignment
User manual placeholder: "Generating Diagrams of Your Project"
