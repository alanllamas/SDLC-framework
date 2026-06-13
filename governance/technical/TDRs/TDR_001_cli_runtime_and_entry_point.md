---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_001
type: TDR
title: "CLI Runtime & Entry Point Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: []
  cross_plane:
    architecture_adr: ADR_002
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_001 — CLI Runtime & Entry Point

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_002 — Runtime Environment & Bootstrap
* **Decision Scope:** Language, runtime, and entry point structure.
* **Boundaries Respected:** Linux/WSL target, adapter pattern mandatory per ADR_002.

## 2. The Implementation Decision
* **Selected Approach:** Python 3.11+ as runtime. Single entry point `main.py`.
  Package managed via `pyproject.toml`.
* **Rationale:** Native LLM SDK support, rich ecosystem, available on all target
  platforms. AREQ_002 adapters map cleanly to Python ABCs.

## 3. Evaluated Alternatives
### Option A — Python 3.11+ (Selected)
* **Pros:** Native LLM SDK, rich ecosystem, readable.
* **Cons:** Slower startup than compiled — acceptable for interactive CLI.

### Option B — Node.js/TypeScript
* **Pros:** Strong async support.
* **Cons:** Additional runtime dependency, less natural for filesystem/Git ops.

## 4. AREQ Boundary Compliance
* Adapter pattern via Python ABCs. Mock adapters as concrete subclasses.
* Adapter Registry via .butler.env. No provider SDK outside implementations.

## 5. Architect Validation
* **Validation required:** NO
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_001, TREQ_002
* **DEV_TICKETs impacted:** All v0.1.0 tickets
