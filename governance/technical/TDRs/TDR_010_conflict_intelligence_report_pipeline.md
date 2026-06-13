---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_010
type: TDR
title: "Conflict Intelligence Report (CIR) Pipeline"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.6.0 — Conflict Intelligence & History"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_006, TDR_007]
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

# Technical Decision Record: TDR_010 — Conflict Intelligence Report (CIR) Pipeline

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How Conflict Intelligence Reports are
  constructed, presented in three tiers, labeled by loop, and
  resolved/escalated per PREQ_007 and BREQ_002/006.

## 2. The Implementation Decision
* **Selected Approach:** CIR reuses the TQUERY pipeline
  (TDR_007) structure — in-memory construction by originating
  agent, Librarian validation and write to
  `/governance/bprops/cirs/pending/`, atomic move to `resolved/`
  on resolution via `archive_asset()`. The CIR payload extends
  the TQUERY pattern with three explicit tiers
  (`the_conflict`, `agent_proposals`, `decision_slot`) and a
  `loop` field (`LOOP_1` | `LOOP_2` | `LOOP_3` per BREQ_006).

  This pipeline **replaces** the interim TQUERY-based conflict
  path established in v0.5.0 TREQ_018 — `detect_conflict()`
  now constructs a CIR payload instead of a TQUERY payload
  when a constraint conflict is detected.

* **Rationale:** CIR and TQUERY share the same lifecycle
  mechanics (freeze → present → resolve → unfreeze → Ledger
  entry) — reusing TDR_007's infrastructure avoids duplicating
  the pending/resolved file management, atomic writes, and
  state.yaml integration. The three-tier structure and loop
  label are CIR-specific additions layered on top.

## 3. Evaluated Alternatives
### Option A — Extend TQUERY pipeline with CIR-specific fields (Selected)
* **Pros:** Reuses proven v0.3.0 infrastructure, single
  pending/resolved pattern to maintain, consistent operator
  mental model (both are "things that block and need my input").
* **Cons:** CIR and TQUERY now share a code path that must
  branch on payload type — mitigated by clean dataclass
  inheritance.

### Option B — Fully separate CIR pipeline
* **Pros:** No shared-code branching.
* **Cons:** Duplicates file management, atomic writes, and
  state.yaml wiring already built for TQUERY.

## 4. AREQ Boundary Compliance
* All file operations via FilesystemAdapter, atomic per TREQ_007.
* CIR presentation fits TDR_003 active artifact budget — long
  proposal sections truncate per existing chunking pattern.

## 5. Architect Validation
* **Validation required:** NO — extension of ADR_004 patterns
  already validated.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_019 (CIR dataclass, three-tier
  construction & loop labeling), TREQ_020 (CIR presentation,
  resolution, escalation & conflict history query)
* **DEV_TICKETs impacted:** CIR pipeline; retrofits v0.5.0
  TREQ_018 conflict path
