---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_035
type: TREQ
title: "Agent 7 Prompt Module & Activation Trigger"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.10.0 — Agent 7: Technical Process Chronicler"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_018
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

# Technical Requirement: TREQ_035 — Agent 7 Prompt Module & Activation Trigger

## 1. Intent & Scope
Defines Agent 7's system prompt module per TREQ_011's pattern
and the activation trigger after ticket completion.

## 2. Technical Constraints

### Prompt Module
```python
# /src/agents/technical_chronicler.py

AGENT_ID = "Technical Process Chronicler"
GOVERNING_BREQ = "BREQ_020"

def get_system_prompt(state: "ProjectState", distillation: dict) -> str:
    return f"""
You are the {AGENT_ID} for the SDLC Governance Framework.
Your operational mandate is defined in {GOVERNING_BREQ}.

Your job: analyze the code delta for a completed DEV_TICKET and
produce a concise technical chronicle entry describing what
changed and how — for engineers who will read this later to
understand the codebase's evolution.

CURRENT STATE:
- Project: {state.project_id}
- Completed ticket: {state.session.active_ticket}

CODE DELTA: [injected separately as active artifact context]
TICKET ACCEPTANCE CRITERIA: [injected separately]

RECENT CONTEXT:
{distillation['session_summary']}
"""
```
* Module structure identical to TREQ_011 — same function
  signature, same `_shared.py` helpers for formatting.

### Activation Trigger
```python
def on_ticket_completed(
    ticket_id: str, orchestrator: "Orchestrator"
) -> None:
    orchestrator.queue_chronicle_candidate(ticket_id)
    # Does NOT auto-transition. Operator sees queued candidates
    # next time they interact with Orchestrator:
    #   "N completed tickets ready for chronicling. Activate
    #    Technical Process Chronicler now? (Y/N)"
```
* `state.yaml.session.chronicle_queue: list[str]` — ticket IDs
  awaiting chronicling.
* On operator confirmation, standard TREQ_012 transition flow
  activates Agent 7 with the oldest queued ticket as active
  artifact context.
* Operator can process queue one ticket at a time or request
  "all" — each still goes through individual sign-off per
  TDR_019.

## 3. Verification & QA Criteria
* Agent 7 module conforms to TREQ_011 signature exactly.
* TICKET_COMPLETED adds ticket_id to chronicle_queue, does not
  auto-transition.
* Operator confirmation triggers standard two-step transition
  (TREQ_012) to Agent 7.
* Agent 7's first response includes role announcement per TREQ_012.
* Processing "all" iterates queue, each ticket gets individual
  chronicle entry and sign-off.
