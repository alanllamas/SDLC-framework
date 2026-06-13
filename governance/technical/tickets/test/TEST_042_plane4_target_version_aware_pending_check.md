# TEST TICKET: TEST_042 — Plane 4 Target-Version-Aware Pending Check
**Parent DEV:** DEV_042 | **Parent TREQ:** TREQ_043

## 1. Test Goal
Verify Plane 4 correctly distinguishes blocking vs
correctly-deferred pending items, and that closure gating
respects this distinction.

## 2. Test Environment
Sample pending TQUERY fixtures: one with no target_version,
one with target_version matching audit_version, one with
target_version for a future version.

## 3. Implementation Plan
* Unit: pending item, no target_version, audit_version="v1.0.0"
  → RED
* Unit: pending item, target_version="v1.0.0",
  audit_version="v1.0.0" → RED
* Unit: pending item, target_version="v2.0.0",
  audit_version="v1.0.0" → GREEN, correctly-deferred message
* Unit: attempt_phase_1_close() with only the GREEN deferred
  item pending → red_count == 0, closure proceeds (assuming
  other planes clean)
* Unit: GREEN deferred item still appears in generated GIR
  report text (visibility, not silently dropped)
* Integration: real TQ_015 (resolved, removed from pending) +
  TQ_016 (target_version="v2.0.0", audit_version="v1.0.0")
  → Plane 4 produces zero RED findings for v1.0.0 audit
