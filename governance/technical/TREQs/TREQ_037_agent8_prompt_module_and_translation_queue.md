---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_037
type: TREQ
title: "Agent 8 Prompt Module & Translation Queue"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.11.0 — Agent 8: Product Delivery Translator"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_020
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

# Technical Requirement: TREQ_037 — Agent 8 Prompt Module & Translation Queue

## 1. Intent & Scope
Defines Agent 8's system prompt module per TREQ_011's pattern
and the translation queue populated from authorized chronicle
entries.

## 2. Technical Constraints

### Prompt Module
```python
# /src/agents/product_translator.py

AGENT_ID = "Product Delivery Translator"
GOVERNING_BREQ = "BREQ_021"

def get_system_prompt(state: "ProjectState", distillation: dict) -> str:
    return f"""
You are the {AGENT_ID} for the SDLC Governance Framework.
Your operational mandate is defined in {GOVERNING_BREQ}.

Your job: translate technical chronicle entries into
plain-language release notes and user manual content —
for stakeholders and end users who don't need implementation
detail, only what changed and how to use it.

CURRENT STATE:
- Project: {state.project_id}
- Pending translations: {len(state.session.translation_queue)}

CHRONICLE ENTRIES: [injected separately as active artifact context]
DOCUMENTATION ASSIGNMENTS: [injected separately, from source DEV_TICKETs]

RECENT CONTEXT:
{distillation['session_summary']}
"""
```
* Module structure identical to TREQ_011/TREQ_035.

### Translation Queue Population
```python
def on_document_authorized(
    artifact_id: str, artifact_type: str, front_matter: dict,
    orchestrator: "Orchestrator"
) -> None:
    if (artifact_type == "CHRONICLE"
            and not front_matter.get("consumed_by_agent_8", True)):
        orchestrator.queue_translation_candidate(artifact_id)
```
* `state.yaml.session.translation_queue: list[str]` — chronicle IDs.
* Operator notification: "N chronicle entries ready for
  translation. Activate Product Delivery Translator now? (Y/N)"
* Confirmation triggers standard TREQ_012 transition.

## 3. Verification & QA Criteria
* Agent 8 module conforms to TREQ_011 signature exactly.
* CHRONICLE sign-off with consumed_by_agent_8=false adds to
  translation_queue.
* CHRONICLE sign-off with consumed_by_agent_8=true (already
  consumed — re-sign-off edge case) does NOT re-queue.
* Operator confirmation triggers standard transition with
  role announcement.
