---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_002
type: TDR
title: "Adapter Base Class & Registry Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_001]
  cross_plane:
    architecture_adr: ADR_001
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_002 — Adapter Base Class & Registry

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_001 — Vendor-Agnostic Architecture
* **Decision Scope:** How adapter interfaces are declared and loaded at runtime.

## 2. The Implementation Decision
* **Selected Approach:** Python ABCs for interface contracts. Factory class
  reads .butler.env and returns correct adapter instance. AdapterResult as dataclass.
* **Rationale:** ABCs enforce compliance at class definition time. Factory keeps
  switching to a single config change. Dataclass provides typed result structure.

## 3. Evaluated Alternatives
### Option A — ABC + Factory (Selected)
* **Pros:** Compile-time enforcement, clean factory pattern, pythonic.
* **Cons:** Slightly more boilerplate than duck typing.

### Option B — Protocol (structural subtyping)
* **Pros:** More flexible, no inheritance required.
* **Cons:** Violations only caught at runtime.

## 4. AREQ Boundary Compliance
* AdapterResult: { success: bool, data: Any|None, error: str|None }
* No provider exceptions propagate beyond adapter boundary.
* Every ABC has corresponding MockAdapter subclass.

## 5. Architect Validation
* **Validation required:** NO
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_003, TREQ_004
* **DEV_TICKETs impacted:** All adapter implementation tickets
