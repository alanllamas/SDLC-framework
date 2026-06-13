---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_017
type: TREQ
title: "Clarification Directive & Fixed-Option Gate Rendering"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.5.0 — Technical Input & Scope Clarification"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_009
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

# Technical Requirement: TREQ_017 — Clarification Directive & Gate Rendering

## 1. Intent & Scope
Defines the structured directive format agents emit when
detecting an ambiguous technology mention, and how the
Orchestrator renders it as a fixed-option gate per PREQ_009
section 2.2.

## 2. Technical Constraints

### Directive Format
Agent system prompts (TREQ_011) instruct: when a technology
is mentioned without a clear necessity keyword (per BREQ_001
list: "must use", "compliance constraint", "contractual
obligation", "strategic necessity"), emit this block at the
end of the response:

```
<<CLARIFICATION_GATE>>
technology: [name]
context: [one sentence — what was said]
<<END_CLARIFICATION_GATE>>
```

### Orchestrator Recognition & Rendering
```python
def render_clarification_gate(directive: dict) -> str:
    return f"""
🔍 DETECTED: You mentioned **{directive['technology']}**.
   ({directive['context']})

Is this a hard requirement or a preference?

A) **Hard Constraint** — non-negotiable, locked downstream
B) **Flexible Preference** — logged for the Architect to evaluate

Reply with A or B.
"""
```
* If `<<CLARIFICATION_GATE>>` block present in agent response,
  Orchestrator strips it from the visible response and renders
  the gate instead — gate becomes the entire turn's output.
* Operator reply restricted to "A" or "B" (case-insensitive,
  trimmed). Any other input → Orchestrator re-presents the gate
  with a one-line reminder, per PREQ_009 Scenario 6 — agent then
  extracts intent from free-text and confirms interpretation
  before re-offering A/B.

## 3. Verification & QA Criteria
* Directive block correctly parsed and stripped from visible response.
* Gate rendered with exactly options A and B — no other text
  besides detection context.
* Reply "A" or "B" (any case) routes correctly.
* Other input triggers re-presentation per Scenario 6.
* If no directive present in response, normal response passes through unchanged.
