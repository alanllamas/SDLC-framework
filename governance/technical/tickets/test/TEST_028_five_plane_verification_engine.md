# TEST TICKET: TEST_028 — Five-Plane Verification Engine
**Parent DEV:** DEV_028 | **Parent TREQ:** TREQ_029

## 1. Test Goal
Verify each plane function in isolation and the combined
audit run against synthetic artifact graphs.

## 2. Test Environment
Synthetic dependency graphs covering each plane's pass/fail/
warning scenarios; sample bprops/*/pending/ directories;
sample Active Constraints and Anti-Goals.

## 3. Implementation Plan
* Unit: plane_1 — valid chain to BDR (no finding) vs broken
  chain (RED)
* Unit: plane_2 — artifact with child (no finding), DEFERRED
  skip without justification (YELLOW), no child/no skip (RED)
* Unit: plane_3 — AUTHORIZED cross-plane ref (no finding) vs
  dangling/non-AUTHORIZED (RED)
* Unit: plane_4 — empty pending dirs (no findings) vs file
  present (RED with correct artifact_id)
* Unit: plane_5 — no anti-goal match (no finding), match
  without CIR (YELLOW), match with unresolved CIR (RED)
* Unit: run_five_plane_audit() calls build_dependency_graph()
  exactly once (call counter mock)
* Integration: real v0.1.0-v0.7.0 artifact set produces zero
  RED findings (or expected ones if intentional gaps remain)
