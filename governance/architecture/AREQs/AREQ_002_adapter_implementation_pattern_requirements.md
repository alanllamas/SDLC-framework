---
extends_schema: "/governance/templates/master_metadata.yaml"
id: AREQ_002
type: AREQ
title: "Adapter Implementation Pattern Requirements"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_006
    parent_id: ADR_001
  horizontal:
    depends_on: [ADR_001, ADR_002, ADR_005]
  cross_plane:
    architecture_adr: [ADR_001]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "System Architect", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Architectural Requirement: AREQ_002 — Adapter Implementation Pattern

## 1. Intent & Scope
* **Description:** Defines the mandatory structural pattern all adapters must
  follow when implementing interfaces declared in ADR_001.
* **Business/Product Rationale:** Without a consistent pattern, adapters are
  non-interchangeable and testing is inconsistent.

## 2. Technical Constraints & Bounds

### 2.1 The Adapter Interface Contract
Every adapter must implement a formally declared interface with:
* Method signatures as declared in ADR_001.
* Strictly typed input parameters.
* Consistent result structure:
```
{ success: bool, data: [payload or null], error: [description or null] }
```
* No provider exceptions propagation — adapters catch internally.

### 2.2 The Mock Adapter Requirement
Every interface must ship with a Mock Adapter:
* Lives in /src/adapters/mocks/ — never mixed with production.
* System fully operable with mock adapters — no live credentials needed.
* Must support configurable failure scenarios per AREQ_001.

### 2.3 Adapter Registry
Registered in .butler.env as environment variables:
```
LLM_ADAPTER=anthropic_claude
GIT_ADAPTER=github
FILESYSTEM_ADAPTER=local_unix
REGISTRY_ADAPTER=local_sdlc
```

### 2.4 Adapter Directory Structure
```
/src/adapters/
├── interfaces/          # Abstract interface contracts
├── implementations/     # Production adapters
└── mocks/              # Mock adapters for testing
```

### 2.5 Violation Detection
Agent 9 verifies at every merge gate:
* No core agent imports provider SDK directly.
* Every integration routes through registered adapter interface.
* Every implementation has corresponding mock.
* Registry references only registered adapters.

## 3. HITL Intelligence & Review Loop
* Language choice left to Tech Lead — pattern applies regardless.
* Interface mechanism (ABC, protocols, duck typing) left to Tech Lead.

## 4. Verification & QA Criteria
* Metric 1: 100% of adapter methods return standard result structure.
* Metric 2: 100% of production adapters have corresponding mock.
* Metric 3: 100% of integration categories registered in .butler.env.
* Metric 4: 0% of core agent files import provider SDKs directly.
