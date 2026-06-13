---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_011
type: TDR
title: "Document History & Impact Analysis Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.6.0 — Conflict Intelligence & History"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_004, TDR_008]
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

# Technical Decision Record: TDR_011 — Document History & Impact Analysis

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How decision history queries are answered
  from the Global History Ledger, and how impact analysis maps
  direct/indirect/safe-zone artifacts before a modification,
  per PREQ_008.

## 2. The Implementation Decision
* **Selected Approach:**
  - **History queries** read structured Ledger entries
    (per BREQ_013's `LEDGER_ENTRY_template.yaml`, written since
    v0.1.0 by every Librarian operation) and render them as a
    reverse-chronological timeline. No new write infrastructure
    needed — this is read-only aggregation over existing entries.
  - **Impact analysis** builds a lightweight in-memory dependency
    graph by scanning `relationships` front-matter
    (`parent_id`, `depends_on`, `architecture_adr`,
    `technical_schema`, `supersedes_document`) across all
    governance files. Given a target document, direct impacts
    are immediate graph neighbors; indirect impacts are
    transitive neighbors up to a configurable depth; everything
    else is safe zone.

* **Rationale:** Every prior milestone (v0.1.0-v0.5.0) already
  generates Ledger entries via Librarian operations — history
  queries are "free" aggregation, not new capture infrastructure.
  Impact analysis reuses the same `master_metadata.yaml`
  relationship fields every artifact already declares — no
  separate dependency-tracking system required.

## 3. Evaluated Alternatives
### Option A — Read-only Ledger aggregation + front-matter graph (Selected)
* **Pros:** Zero new capture infrastructure, graph built from
  data already present in every artifact's front-matter.
* **Cons:** Graph rebuilt on each query — acceptable for v1.0
  repo sizes; caching deferred to Future Horizon if needed.

### Option B — Dedicated dependency database
* **Pros:** Faster queries at scale.
* **Cons:** New infrastructure, sync risk with front-matter,
  over-engineered for v1.0 single-operator repo sizes.

## 4. AREQ Boundary Compliance
* All reads via FilesystemAdapter — Ledger files and governance
  artifacts.
* Graph construction is pure computation — no adapter calls
  beyond initial directory scan.

## 5. Architect Validation
* **Validation required:** NO — read-only aggregation over
  existing ADR_004 structures.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_021 (decision history query),
  TREQ_022 (impact analysis graph)
* **DEV_TICKETs impacted:** History consultation and impact
  analysis features
