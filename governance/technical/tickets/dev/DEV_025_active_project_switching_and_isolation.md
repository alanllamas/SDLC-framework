# DEV TICKET: DEV_025 — Active Project Switching & State Isolation
**version_tag:** v0.7.0 — Innovation Proposal & Project Registry
**Parent TREQ:** TREQ_026
**Trace:** @trace TREQ_026

## 1. Development Process Boundaries
* Implement switch_active_project() per TREQ_026 flush-before-load
  sequence
* Implement registry last_active updates for old and new project
* Implement validation — reject switch to archived/non-existent
  project before any flush side effects are lost

## 2. Acceptance Criteria
* Old project state.yaml and context.md flushed before new
  project loaded
* In-memory state after switch contains only new project's data
* Invalid target rejected, current project state already safely flushed
* Flush failure aborts switch, operator stays on current project
* Both projects' last_active timestamps updated correctly

## 3. Pre-Defined Testing Assignment
See TEST_025

## 4. Documentation Assignment
User manual placeholder: "Switching Between Projects"
