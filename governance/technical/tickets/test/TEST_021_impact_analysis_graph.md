# TEST TICKET: TEST_021 — Impact Analysis Graph
**Parent DEV:** DEV_021 | **Parent TREQ:** TREQ_022

## 1. Test Goal
Verify dependency graph construction and impact map correctness
across direct/indirect/safe-zone classification.

## 2. Test Environment
Synthetic governance artifact set with known relationship
front-matter (parent_id, depends_on, architecture_adr,
technical_schema, supersedes_document chains).

## 3. Implementation Plan
* Unit: graph captures all five reference types correctly
* Unit: impact_map() direct set equals immediate graph neighbors
* Unit: indirect set excludes target and direct, no duplicates,
  respects indirect_depth
* Unit: safe_zone = total - target - direct - indirect, no overlap
* Unit: indirect_depth change via .butler.env alters indirect set
  without code change
* Integration: real ADR_001→AREQ_002→TDR_002 chain produces
  expected direct/indirect classification
