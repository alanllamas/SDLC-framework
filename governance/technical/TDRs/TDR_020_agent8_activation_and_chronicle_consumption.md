---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_020
type: TDR
title: "Agent 8 Activation Trigger & Chronicle Consumption Query"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.11.0 — Agent 8: Product Delivery Translator"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_006, TDR_018, TDR_019]
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

# Technical Decision Record: TDR_020 — Agent 8 Activation Trigger & Chronicle Consumption Query

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_003 — Agent Orchestration Model
* **Decision Scope:** How Agent 8 (Product Delivery Translator,
  mandate per BREQ_021 — GOVERNANCE_POLICY, no PREQ required)
  activates and identifies which chronicle entries to translate
  into stakeholder-facing language. Continues GAP_007.

## 2. The Implementation Decision
* **Selected Approach:** Agent 8 follows the identical
  agent-module + queue + cooling-off-transition pattern
  established for Agent 7 in v0.10.0
  (`/src/agents/product_translator.py`, `BREQ_021`).

  Where Agent 7's queue is populated by `TICKET_COMPLETED`
  events, Agent 8's queue is populated by `DOCUMENT_AUTHORIZED`
  events specifically for `type: CHRONICLE` documents
  (from TDR_008's sign-off flow, TREQ_016) — i.e., whenever a
  chronicle entry becomes AUTHORIZED with
  `consumed_by_agent_8: false`.

  `state.yaml.session.translation_queue: list[str]`
  (chronicle IDs) — populated identically to
  `chronicle_queue` in TREQ_035, same operator-confirmation
  pattern, same "process all" batching.

* **Rationale:** Reusing Agent 7's exact pattern (queue +
  Ledger-event population + operator-confirmed transition)
  means Agent 8 requires zero new orchestration mechanics —
  only a different Ledger event filter
  (`DOCUMENT_AUTHORIZED` + `type: CHRONICLE` +
  `consumed_by_agent_8: false`) and a different agent prompt
  module. Consistent operator mental model: "things waiting for
  an agent" always work the same way.

## 3. Evaluated Alternatives
### Option A — Reuse Agent 7's queue pattern with different event filter (Selected)
* **Pros:** Zero new orchestration mechanics, consistent
  operator experience, minimal new code.
* **Cons:** None significant.

### Option B — Agent 8 runs synchronously as part of chronicle sign-off
* **Pros:** No separate activation step.
* **Cons:** Couples two agents' work into one transaction,
  violates ADR_003 single-active-agent-per-turn model,
  removes operator's ability to batch translations separately
  from chronicling.

## 4. AREQ Boundary Compliance
* No new adapter surface — reuses FilesystemAdapter reads/writes
  via Librarian.

## 5. Architect Validation
* **Validation required:** NO — identical pattern to TDR_018,
  already validated as "just another agent."
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_037 (Agent 8 prompt module &
  translation queue)
* **DEV_TICKETs impacted:** Agent 8 activation
* **Continues:** GAP_007 (Agent 8 portion)
