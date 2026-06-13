# DEV TICKET: DEV_017 — Vision Ledger, Active Constraints & Conflict Scan
**version_tag:** v0.5.0 — Technical Input & Scope Clarification
**Parent TREQ:** TREQ_018
**Trace:** @trace TREQ_018

## 1. Development Process Boundaries
* Implement "Active Technical Constraints" section structure
  in 01_business_request.md per TREQ_018
* Implement Librarian.append_active_constraint()
* Implement Librarian.append_vision_ledger_entry()
* Implement Librarian.read_anti_goals() and
  Librarian.read_active_constraints()
* Implement matches_anti_goal() and detect_conflict() —
  substring/keyword matching, no LLM call
* Implement route_technical_input() orchestration per TREQ_018
* Wire ANTI_GOAL_VIOLATION and CONSTRAINT_CONFLICT response paths
  — interim TQUERY generation for conflicts until v0.6.0 CIR exists

## 2. Acceptance Criteria
* Choice A + anti-goal match → blocked with anti-goal shown + 2 options
* Choice A + constraint conflict → TQUERY generated (interim)
* Choice A + no issues → Active Constraints updated atomically
* Choice B → Vision Ledger updated with PREFERENCE tag atomically
* Anti-goal/conflict scans require no LLM call — pure matching

## 3. Pre-Defined Testing Assignment
See TEST_017

## 4. Documentation Assignment
User manual placeholder: "How Your Tech Choices Get Tracked"
