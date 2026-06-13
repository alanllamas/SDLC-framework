---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_006
type: TDR
title: "Agent System Prompt Loading & Role Transition Logic"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.2.0 — Agent Role Activation & Transitions"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_001, TDR_002, TDR_003, TDR_004]
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

# Technical Decision Record: TDR_006 — Agent System Prompt Loading & Role Transition Logic

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_003 — Agent Orchestration Model
* **Decision Scope:** How agent system prompts are stored/loaded and how
  role transitions execute including cooling-off gate per BREQ_014.

## 2. The Implementation Decision
* **Selected Approach:** Each agent's system prompt is a Python module in
  /src/agents/ exposing get_system_prompt(state, distillation) -> str.
  Role transitions are two-step: Propose (Transition Summary presented
  as cooling-off gate) and Confirm (state.yaml updated, Ledger entry,
  new agent loaded with role announcement).
* **Rationale:** Python modules allow dynamic state injection without
  fragile templating. Two-step transition enforces BREQ_014 structurally.

## 3. Evaluated Alternatives
### Option A — Python modules with templated prompts (Selected)
* **Pros:** Dynamic injection, type-safe, testable.
* **Cons:** More code than static files.

### Option B — Static markdown prompt files
* **Pros:** Simple, human-editable.
* **Cons:** Fragile string interpolation for state injection.

## 4. AREQ Boundary Compliance
* No adapter calls within prompt modules.
* State read via read_context() per ADR_005.

## 5. Architect Validation
* **Validation required:** NO
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_011, TREQ_012
* **DEV_TICKETs impacted:** Agent prompts and Orchestrator transition logic
