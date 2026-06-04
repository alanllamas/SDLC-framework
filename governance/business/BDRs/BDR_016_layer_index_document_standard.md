---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_016
type: BDR
title: "Layer Index Document Standard"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_016
    parent_id: null
  horizontal:
    depends_on: [BDR_001, BDR_011, BDR_015]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Business Decision Record: BDR_016 — Layer Index Document Standard

## 1. Status & Metadata
* **BDR ID:** BDR_016
* **Date/Timestamp:** 2026-06-03 UTC
* **Type:** Core Governance Policy
* **Impacted Scope Tracks:** All governance layers —
  business, product, architecture, technical

## 2. Context & Problem Statement
The framework defines a baseline index document at
the business layer (01_business_request.md) per
BREQ_003, but each subsequent governance layer lacks
an equivalent document to anchor its own context,
track its future horizon informally, and accumulate
intelligence across sessions. Without this, agents
start each session without a structured baseline,
and future horizon ideas have no formal home at
the layer where they surface — forcing incorrect
cross-layer document modifications.

The global artifact index across all layers is
already managed by SYSTEM_REGISTRY.md. The Layer
Index Document is not a replacement for the registry
— it is a layer-specific intelligence anchor that
complements it.

## 3. The Business Decision (The "What")

### 3.1 Mandatory Layer Index Document
Every governance layer must maintain a dedicated
Layer Index Document as its structural anchor.
The four mandatory Layer Index Documents are:

* **Business Layer:** `01_business_request.md`
  — already established per BREQ_003. Owned
  exclusively by the Business Catalyst.
* **Product Layer:** `01_product_baseline.md`
  — lives in `/governance/product/`. Owned
  exclusively by the PM Orchestrator.
* **Architecture Layer:** `01_architecture_baseline.md`
  — lives in `/governance/architecture/`. Owned
  exclusively by the System Architect.
* **Technical Layer:** `01_technical_baseline.md`
  — lives in `/governance/technical/`. Owned
  exclusively by the Tech Lead.

### 3.2 Mandatory Content Structure
Every Layer Index Document must contain the following
sections at minimum:

* **Layer Mission:** A maximum 3-sentence statement
  of what this layer is responsible for producing
  and what it explicitly does not own.

* **Active Artifacts Index:** A running list of all
  authorized artifacts in this layer with their
  current status and file paths. This is a
  layer-scoped view — the global view lives in
  SYSTEM_REGISTRY.md.

* **Accumulated Intelligence Ledger:** A structured
  log of key decisions, patterns, and constraints
  discovered during sessions that are not captured
  in individual artifacts but are valuable for
  future sessions in this layer.

* **Future Horizon Ledger:** An informal, unordered
  capture of ideas, improvements, and innovations
  surfaced during sessions that are not in scope
  for the current planning batch. These are raw
  ideas — not commitments, not backlog, not
  prioritized work. When an idea in this ledger
  matures and the HITL decides to activate it,
  it exits the ledger and enters the formal planning
  process as a new artifact (BREQ, PREQ, ADR, etc.)
  at the appropriate layer. Until then it has no
  priority, no date, and no formal status.

* **Inherited TQUERYs:** A list of TQUERYs passed
  down from upstream layers or escalated from
  downstream layers that this layer is responsible
  for resolving.

### 3.3 Layer Sovereignty & Cross-Layer Rules
* Each Layer Index Document is under the exclusive
  editorial authority of its owning agent. No other
  agent may request write, modify, or append
  operations on another layer's index document.
  All physical write operations are always executed
  exclusively by the Librarian per BREQ_010.
* If an agent captures a Future Horizon idea that
  belongs to or impacts a higher layer, it must
  route it upward via the Upstream Innovation
  Reflow Protocol (BREQ_009) — never by directly
  modifying another layer's index document.
* If a conflict arises between adjacent layers
  regarding a Future Horizon item, it is resolved
  via the Bidirectional Conflict Protocol (BREQ_006)
  before any formal artifact is created.
* The normal downstream pipeline (Business → Product
  → Architecture → Technical) governs how authorized
  artifacts flow between layers. The Layer Index
  Document is never part of that pipeline — it is
  an internal intelligence anchor for its owning
  layer only.

### 3.4 Initialization Protocol
* A Layer Index Document must be initialized by its
  owning agent at the start of that layer's first
  session for any given project.
* The Git Butler provisions the document on a
  dedicated branch before the agent begins drafting.
* The Layer Index Document is a living document
  — it is never sealed, never marked AUTHORIZED,
  and never superseded. It evolves throughout the
  project lifecycle via append-only operations
  managed exclusively by the Librarian.

## 4. Strategic Rationale & Consequences (The "Why")
* **Context Continuity:** Each layer maintains its
  own intelligence across sessions — agents never
  start from zero.
* **Layer Sovereignty:** Cross-layer document
  modifications are structurally impossible —
  each layer owns its own context exclusively,
  with the Librarian as the sole physical writer.
* **Future Horizon Clarity:** Ideas are captured
  informally at the layer where they surface.
  They are explicitly not backlog — they are a
  safety net for valuable thoughts that are not
  ready for formal planning. Activation happens
  only by explicit HITL decision.
* **Accumulated Intelligence:** Patterns, constraints,
  and discoveries that don't fit in individual
  artifacts have a formal home, preventing knowledge
  loss between sessions.
* **Protocol Reuse:** Cross-layer idea routing uses
  BREQ_009. Cross-layer conflict resolution uses
  BREQ_006. No new protocols are required.
* **Registry Complement:** SYSTEM_REGISTRY.md remains
  the global artifact index. Layer Index Documents
  provide layer-scoped intelligence that the registry
  does not capture.
