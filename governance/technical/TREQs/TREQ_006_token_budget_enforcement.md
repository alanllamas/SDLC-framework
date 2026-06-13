---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_006
type: TREQ
title: "Token Budget Enforcement"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_003
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

# Technical Requirement: TREQ_006 — Token Budget Enforcement

## 1. Intent & Scope
Orchestrator must enforce token budget per TDR_003 before
every LLM call.

## 2. Technical Constraints
* Token counting via tiktoken or provider-equivalent before every call.
* Budget allocation:
    * System prompt: 2,000 tokens max
    * State summary: 500 tokens max
    * Context distillation: 1,500 tokens max
    * Recent exchanges (last 10): 3,000 tokens max
    * Active artifact: 2,000 tokens max
    * Operator input: 500 tokens max
    * Hard ceiling: 9,500 tokens total
* Truncation from least recent content first — never system prompt or input.
* Budget configurable via .butler.env.

## 3. Verification & QA Criteria
* 0% of LLM calls exceed 9,500 tokens.
* Truncation events logged with component and tokens removed.
* Budget configurable without code changes.
