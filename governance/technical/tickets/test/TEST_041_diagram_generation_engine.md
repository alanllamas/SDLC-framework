# TEST TICKET: TEST_041 — Diagram Generation Engine
**Parent DEV:** DEV_041 | **Parent TREQ:** TREQ_042

## 1. Test Goal
Verify diagram generation across all scopes, loop edge
rendering, and Mermaid syntax validity.

## 2. Test Environment
Synthetic dependency graph spanning all four layers, sample
resolved CIRs with LOOP_1/2/3 values.

## 3. Implementation Plan
* Unit: scope="full" node set equals all graph artifacts
* Unit: scope="layer:Technical" includes only TDR/TREQ/DEV/TEST/
  CHRONICLE plus one-hop cross-layer neighbors
* Unit: scope="artifact:[ID]" node set equals impact_map()
  direct+indirect+target
* Unit: resolved CIR LOOP_2 between two artifacts → dashed
  edge with "LOOP_2" label
* Unit: output parses as valid Mermaid graph TD (syntax check
  via mermaid-cli or regex structure validation)
* Unit: output written to governance/diagrams/ with correct
  slug+timestamp naming, no status field in front-matter
* Integration: /diagram command available with
  phase_gates.phase_1_closed=false
