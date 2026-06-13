---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_004
type: TDR
title: "State Machine File Format & Write Protocol"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_001
  horizontal:
    depends_on: [TDR_001, TDR_002, TDR_003]
  cross_plane:
    architecture_adr: ADR_004
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_004 — State Machine File Format & Write Protocol

## 1. Decision Context
* **Parent AREQ:** AREQ_001 — Offline & Connectivity Resilience
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How state.yaml is read, written, and kept consistent.

## 2. The Implementation Decision
* **Selected Approach:** PyYAML for read/write. Atomic writes via temp file +
  rename pattern. State loaded into typed Python dataclass at session start.
  Mutations in-memory, flushed after every significant transition.
* **Rationale:** Human-readable, atomic safety, standard library adjacent.

## 3. Evaluated Alternatives
### Option A — PyYAML + atomic writes (Selected)
* **Pros:** Human-readable, atomic safety.
* **Cons:** No built-in schema validation — mitigated by Librarian per BREQ_010.

### Option B — JSON + atomic writes
* **Pros:** Faster parsing.
* **Cons:** Less human-readable, harder to inspect manually.

## 4. AREQ Boundary Compliance
* Atomic write pattern guarantees state preservation per AREQ_001 section 2.5.
* state.yaml readable without LLM — pure filesystem operation.

## 5. Architect Validation
* **Validation required:** NO
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_007, TREQ_008
* **DEV_TICKETs impacted:** State machine implementation tickets
