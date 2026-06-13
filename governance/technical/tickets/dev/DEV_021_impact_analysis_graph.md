# DEV TICKET: DEV_021 — Impact Analysis Graph
**version_tag:** v0.6.0 — Conflict Intelligence & History
**Parent TREQ:** TREQ_022
**Trace:** @trace TREQ_022

## 1. Development Process Boundaries
* Implement build_dependency_graph() per TREQ_022 — scans all
  governance artifacts' relationship fields
* Implement impact_map() — direct/indirect/safe_zone per
  configurable indirect_depth (default 2, via .butler.env)
* Implement three-section presentation per PREQ_008 section 2.2

## 2. Acceptance Criteria
* Graph captures parent_id, depends_on, architecture_adr,
  technical_schema, and supersedes_document references
* impact_map() direct matches immediate neighbors exactly
* indirect excludes target and direct, no duplicates
* safe_zone = total minus target minus direct minus indirect
* indirect_depth configurable without code changes

## 3. Pre-Defined Testing Assignment
See TEST_021

## 4. Documentation Assignment
User manual placeholder: "Understanding Change Impact"
