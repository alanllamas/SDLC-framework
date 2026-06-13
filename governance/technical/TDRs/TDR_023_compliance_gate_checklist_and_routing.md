---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_023
type: TDR
title: "Compliance Gate Checklist, Report & Outcome Routing"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_022, TDR_027]
  cross_plane:
    architecture_adr: ADR_004
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_023 — Compliance Gate Checklist, Report & Outcome Routing

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** What Agent 9 checks before a ticket's PR
  is created, how findings are reported, and what happens on
  PROCEED vs HOLD per BDR_005.

## 2. The Implementation Decision
* **Selected Approach:** Agent 9's checklist covers four checks,
  each producing a Green/Yellow/Red finding (reusing TREQ_029's
  `Finding` model and severity vocabulary for consistency with
  the GIR engine):

  1. **Test Results** — `run_tests()` (TREQ_039) against the
     ticket's TEST_TICKET path. Failures → RED.
  2. **Chronicle Status** — is a chronicle entry for this ticket
     AUTHORIZED or at least queued (TREQ_035)? Missing entirely
     → YELLOW (informational — doesn't block, Agent 7 may run
     later).
  3. **Commit Compliance** — do all commits on the branch follow
     the `[ticket_id]` prefix per TREQ_032? Non-conforming
     commits → YELLOW.
  4. **Mini Coverage Check** — for any NEW governance artifacts
     introduced during this ticket's work (e.g., a TQUERY
     resolution that produced a new artifact), re-run plane 3
     (horizontal coverage, TREQ_029) scoped to just those new
     artifacts. Dangling reference → RED.

  Agent 9 renders a **Compliance Report** (same structure as a
  GIR finding list, per TREQ_030's format) and presents two
  options:
  - **A) Proceed** — continue `complete_ticket()` to PR creation
    (TREQ_032), regardless of findings (HITL sovereignty —
    Red findings shown but don't auto-block).
  - **B) Hold** — pause completion; Orchestrator offers to route
    Red findings to TQUERY (test failures, dangling references)
    or simply return to Execution Mode for the operator to fix
    before re-running Agent 9.

* **Rationale:** Reusing `Finding`/severity vocabulary from
  TREQ_029 means Agent 9's report and the GIR (TDR_015) share a
  visual/structural language — operators learn one severity
  system. HITL-decides-on-Red preserves BDR_005 while still
  making Red findings impossible to miss. Routing Red findings
  to TQUERY on Hold reuses TDR_007 rather than inventing a new
  "fix-it" flow.

## 3. Evaluated Alternatives
### Option A — Four-check report reusing Finding/severity model, HITL chooses Proceed/Hold (Selected)
* **Pros:** Consistent severity vocabulary with GIR, HITL
  sovereignty preserved, Hold path reuses TQUERY pipeline.
* **Cons:** None significant.

### Option B — Automatic hard block on any Red finding
* **Pros:** Stronger automated guarantee.
* **Cons:** Violates BDR_005 — HITL must retain final
  authority over their own workflow, especially for a
  Developer Solo operator who may have valid reasons
  (e.g., known-flaky test, WIP commit) to proceed anyway.

## 4. AREQ Boundary Compliance
* Mini coverage check reuses TREQ_022/029 graph functions —
  no new adapter calls beyond TestRunnerAdapter (TREQ_039).

## 5. Architect Validation
* **Validation required:** NO — reuses TREQ_029/030 vocabulary
  and TDR_007 TQUERY routing, both already authorized.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_040 (Agent 9 prompt module,
  compliance report generation & gate decision routing)
* **DEV_TICKETs impacted:** Agent 9 implementation
* **Completes:** GAP_005 — all three documentation/compliance
  agents (7, 8, 9) operational. v1.0.0 Framework Complete scope
  fully specified.
