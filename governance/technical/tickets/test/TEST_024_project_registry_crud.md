# TEST TICKET: TEST_024 — Project Registry CRUD Operations
**Parent DEV:** DEV_024 | **Parent TREQ:** TREQ_025

## 1. Test Goal
Verify registry table parsing and CRUD operations including
non-destructive archival.

## 2. Test Environment
Temporary ~/.sdlc/project_registry.md with sample rows.

## 3. Implementation Plan
* Unit: list_projects() parses existing table rows correctly
* Unit: register_project() assigns next sequential PRJ_ID,
  no collision
* Unit: register_project() creates valid state.yaml matching
  ADR_004 schema and empty context.md
* Unit: archive_project() sets status=ARCHIVED, directory remains
* Unit: list_projects() excludes ARCHIVED by default,
  includes with include_archived=True
