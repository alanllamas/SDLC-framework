---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_003
type: TDR
title: "Orchestrator & Context Assembly Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_001
  horizontal:
    depends_on: [TDR_001, TDR_002]
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

# Technical Decision Record: TDR_003 — Orchestrator & Context Assembly

## 1. Decision Context
* **Parent AREQ:** AREQ_001 — Offline & Connectivity Resilience
* **Governing ADR:** ADR_003 — Agent Orchestration Model
* **Decision Scope:** Context assembly and conversation history management.

## 2. The Implementation Decision
* **Selected Approach:** Context Distillation pattern. Raw history capped at
  last 10 exchanges. Full history in ~/.sdlc/ — never auto-loaded, on demand only.
* **Token budget:** System prompt 2k + State 500 + Distillation 1.5k +
  Exchanges 3k + Artifact 2k + Input 500 = 9,500 tokens hard ceiling.

## 3. Evaluated Alternatives
### Option A — Context Distillation + Hard Cap (Selected)
* **Pros:** Predictable token usage, no overflow, full history on demand.
* **Cons:** Agent may miss early context — mitigated by distillation quality.

### Option B — Sliding Window
* **Pros:** Simple.
* **Cons:** Unpredictable usage, important decisions may fall off.

## 4. AREQ Boundary Compliance
* State auto-preserved to ~/.sdlc/ on every significant exchange.
* Full history accessible offline — no LLM required.
* API failure detection per AREQ_001 sections 2.1 and 2.5.

## 5. Architect Validation
* **Validation required:** YES — token budget impacts all agent interactions.
* **Architect sign-off:** Alan Llamas via Console Approval
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_005, TREQ_006
* **DEV_TICKETs impacted:** Orchestrator implementation tickets
