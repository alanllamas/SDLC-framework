# DEV TICKET: DEV_042 — Plane 4 Target-Version-Aware Pending Check
**version_tag:** v0.12.0 — Agent 9: Compliance Gatekeeper
**Parent TREQ:** TREQ_043
**Trace:** @trace TREQ_043

## 1. Development Process Boundaries
* Add optional `target_version` field to
  governance/templates/TQUERY_metadata.yaml (additive,
  defaults null)
* Refactor plane_4_pending_resolution() per TREQ_043 — accept
  audit_version param, classify pending items RED vs GREEN
  based on target_version comparison
* Wire run_five_plane_audit() to pass
  state.yaml.session.current_version as audit_version
* Update attempt_phase_1_close() (TREQ_030) — only count
  RED/YELLOW from Plane 4 toward red_count; GREEN deferred
  items appear in report but don't block

## 2. Acceptance Criteria
* Pending item with no target_version → RED (unchanged default)
* Pending item with target_version == audit_version → RED
* Pending item with target_version != audit_version → GREEN,
  marked correctly-deferred, doesn't block
* attempt_phase_1_close() red_count excludes GREEN deferred items
* TQUERY_metadata.yaml accepts optional target_version without
  breaking existing TQUERYs lacking the field

## 3. Pre-Defined Testing Assignment
See TEST_042

## 4. Documentation Assignment
User manual placeholder: "Understanding Deferred Items in the Integrity Audit"
