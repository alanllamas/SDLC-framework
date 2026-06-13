---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_012
type: TREQ
title: "Role Transition & Cooling-Off Gate Flow"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.2.0 — Agent Role Activation & Transitions"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_006
  cross_plane:
    architecture_adr: ADR_003
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_012 — Role Transition & Cooling-Off Gate Flow

## 1. Intent & Scope
Defines the two-step state machine flow for transitioning between
active agents per BREQ_014 cooling-off gate.

## 2. Technical Constraints

### Step 1 — Propose Transition
* Triggered at natural boundary or explicit operator request.
* Orchestrator generates Transition Summary:
```yaml
transition_summary:
  from_agent: "[current AGENT_ID]"
  to_agent: "[proposed AGENT_ID]"
  reason: "[why]"
  accomplished_this_session:
    - "[artifact or decision]"
  next_agent_will: "[what comes next]"
```
* Presented to operator — state.yaml NOT updated yet. This is the
  cooling-off gate.

### Step 2 — Confirm Transition
* On confirmation:
    1. Librarian updates state.yaml.active_agent and active_layer
       atomically per TREQ_007.
    2. Global History Ledger entry generated per BREQ_013.
    3. New agent's get_system_prompt() called.
    4. New agent's first response begins with role announcement:
       "[Agent Name] activo. [One sentence on what comes next]."
* On rejection: no state change.

### 2.1 Direct Agent Invocation Redirect
Per BREQ_017 section 5, direct invocation without transition flow
is intercepted and redirected through Step 1.

## 3. Verification & QA Criteria
* 100% of transitions present Transition Summary before state change.
* state.yaml never updated without operator confirmation.
* Every confirmed transition generates Ledger entry.
* New agent's first response contains role announcement.
* Direct invocation always redirected through gate.
