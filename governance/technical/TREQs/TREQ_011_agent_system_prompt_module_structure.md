---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_011
type: TREQ
title: "Agent System Prompt Module Structure"
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

# Technical Requirement: TREQ_011 — Agent System Prompt Module Structure

## 1. Intent & Scope
Defines the structure every agent system prompt module must follow
in /src/agents/.

## 2. Technical Constraints
```python
# /src/agents/business_catalyst.py

AGENT_ID = "Business Catalyst"
GOVERNING_BREQ = "BREQ_004"

def get_system_prompt(state: "ProjectState", distillation: dict) -> str:
    return f"""
You are the {AGENT_ID} for the SDLC Governance Framework.
Your operational mandate is defined in {GOVERNING_BREQ}.

CURRENT STATE:
- Project: {state.project_id}
- Phase: {state.active_phase}
- Layer: {state.active_layer}
- Milestone: {state.active_milestone}

RECENT CONTEXT:
{distillation['session_summary']}

KEY DECISIONS THIS SESSION:
{format_decisions(distillation['key_decisions'])}

OPEN QUESTIONS:
{format_questions(distillation['open_questions'])}
"""
```
* One module per agent: business_catalyst.py, pm_orchestrator.py,
  system_architect.py, tech_lead.py.
* Each declares AGENT_ID and GOVERNING_BREQ constants.
* get_system_prompt() signature identical across modules.
* Helper functions shared in /src/agents/_shared.py.

## 3. Verification & QA Criteria
* All four modules implement identical function signature.
* Output stays within TDR_003 system prompt budget (2,000 tokens).
* No adapter calls within agent modules.
