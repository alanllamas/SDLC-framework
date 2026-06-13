---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_012
type: TDR
title: "BPROP Pipeline & Upstream Innovation Reflow"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.7.0 — Innovation Proposal & Project Registry"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_006, TDR_007, TDR_010]
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

# Technical Decision Record: TDR_012 — BPROP Pipeline & Upstream Innovation Reflow

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How a BPROP (innovation proposal originating
  at any layer) travels upstream through enrichment stops to
  Business Catalyst for adjudication, per BREQ_009 and PREQ_010
  section 2.1.

## 2. The Implementation Decision
* **Selected Approach:** A BPROP is constructed by its originating
  agent as a typed payload, written by the Librarian to
  `/governance/bprops/pending/` (the existing `bprops/` root
  established in ADR_004 — BPROPs share this root with TQUERY/CIR
  pending/resolved subdirectories per AREQ_001 directory layout).
  Unlike TQUERY/CIR (single resolution stop), a BPROP carries an
  `enrichment_chain` list. As the proposal is surfaced during each
  intervening layer's session (Tech Lead → Architect → PM, in that
  upstream order), that layer's agent appends an enrichment entry
  via `Librarian.enrich_bprop()` — an in-place atomic update, not
  a pending→resolved move. Only Business Catalyst's adjudication
  triggers the final `archive_asset()` move to `resolved/`.

* **Rationale:** Reusing the `bprops/` pending/resolved root and
  atomic-write infrastructure from TDR_007/TDR_010 avoids a third
  parallel file-management implementation. The enrichment chain as
  in-place updates (rather than a new file per stop) keeps the
  full proposal history in one document — consistent with
  BREQ_009's requirement that each layer's perspective be visible
  to the Business Catalyst at adjudication time.

## 3. Evaluated Alternatives
### Option A — Single BPROP file with in-place enrichment chain (Selected)
* **Pros:** One document carries full history, reuses
  pending/resolved infrastructure, atomic updates per stop.
* **Cons:** File grows with each enrichment — bounded by number
  of layers (max 3 stops), negligible size impact.

### Option B — Separate enrichment file per layer
* **Pros:** Smaller individual files.
* **Cons:** Fragments proposal history across files, requires
  assembly logic at adjudication time — unnecessary complexity
  for max 3 stops.

## 4. AREQ Boundary Compliance
* All operations via FilesystemAdapter, atomic per TREQ_007.
* BPROP presentation fits TDR_003 active artifact budget —
  enrichment chain entries are concise (per TREQ_023 field limits).

## 5. Architect Validation
* **Validation required:** NO — extension of ADR_004 bprops/
  structure already established.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_023 (BPROP dataclass & enrichment
  chain), TREQ_024 (adjudication & routing to governance artifacts)
* **DEV_TICKETs impacted:** BPROP pipeline implementation
