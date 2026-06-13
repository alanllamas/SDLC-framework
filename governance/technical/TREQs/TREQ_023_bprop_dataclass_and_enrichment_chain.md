---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_023
type: TREQ
title: "BPROP Dataclass & Enrichment Chain"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.7.0 — Innovation Proposal & Project Registry"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_012
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_023 — BPROP Dataclass & Enrichment Chain

## 1. Intent & Scope
Defines the BPROP payload structure and the in-place enrichment
operation each intervening layer performs as the proposal
travels upstream per BREQ_009.

## 2. Technical Constraints

```python
from pydantic import BaseModel
from typing import Literal

class EnrichmentEntry(BaseModel):
    layer: Literal["Technical", "Architecture", "Product"]
    agent: str
    notes: str  # max ~100 words — concise perspective, not a rewrite
    timestamp: str

class BPropPayload(BaseModel):
    id: str  # e.g., "BPROP_001" — Librarian assigns sequentially
    originating_layer: Literal["Technical", "Architecture", "Product", "Business"]
    originating_agent: str
    title: str
    proposal_summary: str  # the idea, plain language
    rationale: str  # why it matters
    enrichment_chain: list[EnrichmentEntry] = []
    adjudication: dict | None = None  # populated by Business Catalyst
```

* `Librarian.write_bprop(payload)`:
    1. Validates payload, assigns sequential BPROP_ID.
    2. Atomic write to `governance/bprops/pending/BPROP_NNN_[slug].yaml`.
    3. Updates `state.yaml.session.pending_bprops` (new array).
* `Librarian.enrich_bprop(bprop_id, entry: EnrichmentEntry)`:
    1. Reads pending BPROP file.
    2. Appends `entry` to `enrichment_chain`.
    3. Atomic rewrite of same file (in-place update — no move).
* Upstream order enforced: a BPROP originating at Technical layer
  must collect entries from Architecture and Product (in that
  order) before reaching Business Catalyst. A BPROP originating
  at Architecture skips the Technical stop. Order derived from
  `originating_layer` — Orchestrator presents the BPROP for
  enrichment only to layers strictly above `originating_layer`
  in BDR_001's layer hierarchy, in ascending order.

## 3. Verification & QA Criteria
* BPROP_ID sequential, no collisions across pending/resolved.
* enrich_bprop() is in-place atomic rewrite — file path unchanged,
  enrichment_chain grows by exactly one entry per call.
* Enrichment order matches layer hierarchy above originating_layer.
* state.yaml.session.pending_bprops updated atomically on write.
