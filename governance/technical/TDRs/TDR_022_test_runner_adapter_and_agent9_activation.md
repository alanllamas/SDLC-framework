---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_022
type: TDR
title: "Test Runner Adapter & Agent 9 Activation Trigger"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_006, TDR_016, TDR_018]
  cross_plane:
    architecture_adr: ADR_001
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_022 — Test Runner Adapter & Agent 9 Activation Trigger

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_001 — Vendor-Agnostic Architecture via
  Abstraction Layers & Adapters
* **Decision Scope:** How Agent 9 (Lifecycle Compliance
  Gatekeeper, mandate per BREQ_022 — GOVERNANCE_POLICY, no PREQ
  required) executes the project's test suite, and when it
  activates relative to the ticket completion flow established
  in v0.9.0 (TREQ_032).

## 2. The Implementation Decision
* **Selected Approach:** A new `TestRunnerAdapter` ABC is added
  per AREQ_002's adapter pattern — `run_tests(test_path: str) ->
  AdapterResult` where `data` contains `{passed: int, failed: int,
  output: str}`. The production implementation
  (`PytestRunnerAdapter`) shells out to `pytest` via subprocess
  — consistent with the framework's Python 3.11+ runtime (TDR_001)
  and requiring zero new dependencies beyond `pytest` itself
  (already implied by every TEST_TICKET since v0.1.0). A
  `MockTestRunnerAdapter` returns configurable pass/fail results
  per TREQ_004.

  **Activation:** Agent 9 inserts itself into `complete_ticket()`
  (TREQ_032) as a **mandatory pre-PR step** — when the operator
  signals ticket completion, the Orchestrator transitions to
  Agent 9 (via TREQ_012, cooling-off still applies) BEFORE the PR
  creation step. Agent 9 is not optional/queued like Agents 7-8
  — it is part of the completion sequence itself, but per BDR_005
  the HITL retains final say on whether to proceed (see TDR_023).

* **Rationale:** Shelling out to `pytest` is the simplest possible
  implementation — every TEST_TICKET since v0.1.0 already
  describes pytest-style unit/integration tests
  ("Unit: ...", "Integration: ..."), so the test suite Agent 9
  runs is exactly what TEST_TICKETs have been specifying all
  along. Making Agent 9 mandatory-but-advisory (HITL decides)
  rather than auto-blocking preserves BDR_005 sovereignty while
  still surfacing compliance information at the right moment —
  before code ships, not after.

## 3. Evaluated Alternatives
### Option A — TestRunnerAdapter (pytest subprocess) + mandatory pre-PR activation, HITL decides (Selected)
* **Pros:** Zero new dependencies beyond pytest (already implied),
  reuses adapter pattern, preserves BDR_005 sovereignty,
  activation point aligns with existing completion flow.
* **Cons:** v1.0 only supports pytest — acceptable, Python-only
  framework per TDR_001.

### Option B — CI/CD integration (GitHub Actions) for test execution
* **Pros:** Tests run in clean environment, standard practice.
* **Cons:** New infrastructure dependency (CI config, GitHub
  Actions runners), contradicts ADR_002's "no additional
  infrastructure" v1.0 stance, over-engineered for Developer
  Solo profile.

## 4. AREQ Boundary Compliance
* `TestRunnerAdapter` follows TREQ_003 ABC pattern exactly —
  `AdapterResult` return, mock per TREQ_004.
* Subprocess execution wrapped — exceptions translated to
  `AdapterResult(success=False, error=...)`, never raw propagation.

## 5. Architect Validation
* **Validation required:** YES — new adapter type (first since
  AREQ_002 was authorized) warrants Architect confirmation that
  subprocess-based execution doesn't violate any v1.0 runtime
  boundary.
* **Architect sign-off:** Alan Llamas via Console Approval
* **Loop 3 triggered:** NO — confirmed within existing
  Linux/WSL + Python boundary, no new external dependency.

## 6. Downstream Impact
* **TREQs generated:** TREQ_039 (TestRunnerAdapter
  implementation), TREQ_040 (Agent 9 prompt module, compliance
  report & gate decision)
* **DEV_TICKETs impacted:** Agent 9 implementation, retrofits
  TREQ_032 complete_ticket() sequence
* **Resolves:** GAP_005 (Agent 9 portion of documentation
  agents planning — completes the trio with Agents 7/8)
