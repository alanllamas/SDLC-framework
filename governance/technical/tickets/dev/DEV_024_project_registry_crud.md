# DEV TICKET: DEV_024 — Project Registry CRUD Operations
**version_tag:** v0.7.0 — Innovation Proposal & Project Registry
**Parent TREQ:** TREQ_025
**Trace:** @trace TREQ_025

## 1. Development Process Boundaries
* Implement ProjectEntry model and project_registry.md table
  structure per TREQ_025
* Implement list_projects(), register_project(), archive_project()
* register_project() creates ~/.sdlc/projects/[PRJ_ID]/ with
  initial state.yaml (ADR_004 schema) and empty context.md

## 2. Acceptance Criteria
* list_projects() parses table correctly into ProjectEntry list
* register_project() sequential ID, no collision, valid initial
  state.yaml
* archive_project() sets status=ARCHIVED — never deletes directory
* Archived projects excluded from default list_projects()

## 3. Pre-Defined Testing Assignment
See TEST_024

## 4. Documentation Assignment
User manual placeholder: "Managing Multiple Projects"
