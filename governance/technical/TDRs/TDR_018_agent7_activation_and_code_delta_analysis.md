---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_018
type: TDR
title: "Agent 7 Activation Trigger & Code Delta Analysis"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.10.0 — Agent 7: Technical Process Chronicler"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_006, TDR_016]
  cross_plane:
    architecture_adr: ADR_003
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_018 — Agent 7 Activation Trigger & Code Delta Analysis

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_003 — Agent Orchestration Model
* **Decision Scope:** How Agent 7 (Technical Process Chronicler,
  mandate per BREQ_020 — GOVERNANCE_POLICY, no PREQ required)
  activates after ticket completion, and how it analyzes the
  resulting code delta. Resolves part of GAP_007.

## 2. The Implementation Decision
* **Selected Approach:** Agent 7 follows the same system-prompt
  module pattern as the four Phase 1 agents (TREQ_011) —
  `/src/agents/technical_chronicler.py` with identical
  `get_system_prompt(state, distillation)` signature, governed
  by `BREQ_020`.

  Activation trigger: when `complete_ticket()` (TREQ_032)
  generates a `TICKET_COMPLETED` Ledger event, the Orchestrator
  proposes a transition to Agent 7 via the existing two-step
  cooling-off flow (TDR_006/TREQ_012) — **not automatic**, the
  operator confirms or defers (can batch multiple completed
  tickets before invoking Agent 7).

  Code delta analysis reuses `GitAdapter.fetch_code_delta()`
  (already implemented in v0.1.0 DEV_004) to retrieve the diff
  for the ticket's branch against its base. Agent 7's prompt
  includes this diff plus the DEV_TICKET's acceptance criteria
  as its active artifact context.

* **Rationale:** No new adapter methods needed —
  `fetch_code_delta()` already exists. Reusing the agent-module
  and transition patterns means Agent 7 is "just another agent"
  from the Orchestrator's perspective — zero special-casing in
  core orchestration logic.

## 3. Evaluated Alternatives
### Option A — Standard agent module + existing transition + fetch_code_delta() (Selected)
* **Pros:** Zero new adapter methods, zero orchestration
  special-casing, operator controls timing (batch-friendly).
* **Cons:** None significant.

### Option B — Automatic background invocation on every commit
* **Pros:** Always up-to-date documentation.
* **Cons:** Violates ADR_003 single-active-agent model, removes
  operator control, costly (LLM call per commit).

## 4. AREQ Boundary Compliance
* `fetch_code_delta()` already AREQ_002-compliant from v0.1.0.
* No new adapter surface.

## 5. Architect Validation
* **Validation required:** NO — Agent 7 is "just another agent"
  per ADR_003, no orchestration model change.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_035 (Agent 7 prompt module &
  activation trigger), TREQ_036 (code delta analysis & chronicle
  entry generation)
* **DEV_TICKETs impacted:** Agent 7 implementation
* **Partially resolves:** GAP_007 (Agent 7 portion)
