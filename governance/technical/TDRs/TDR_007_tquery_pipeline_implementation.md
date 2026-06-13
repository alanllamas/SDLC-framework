---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_007
type: TDR
title: "TQUERY Pipeline Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.3.0 — TQUERY Flow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_002, TDR_004, TDR_006]
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

# Technical Decision Record: TDR_007 — TQUERY Pipeline Implementation

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How TQUERYs are constructed, presented,
  resolved, and persisted per BREQ_012 and PREQ_003.
* **Boundaries Respected:** Librarian as exclusive writer per
  BREQ_010/012. Atomic writes per TREQ_007. State transitions
  per ADR_004 section 3.3.

## 2. The Implementation Decision
* **Selected Approach:** Any agent constructs a TQUERY payload
  in-memory as a typed dataclass matching the TQUERY_metadata.yaml
  schema. The agent hands the payload to the Librarian, which
  validates it (per TREQ_008-style validation), writes it to
  `/governance/bprops/tqueries/pending/`, and updates
  `state.yaml.session.pending_tqueries`. The Orchestrator then
  presents the TQUERY to the operator using a fixed presentation
  template — never raw YAML.

  Resolution flow: operator selects an option (or provides custom
  input). Orchestrator writes the `hitl_decision` block back via
  Librarian, moves the file from `pending/` to `resolved/` using
  `archive_asset()` (this is a move, not deletion — compliant with
  BDR_020), removes the TQUERY ID from
  `state.yaml.session.pending_tqueries`, and unfreezes the branch.

* **Rationale:** Keeping TQUERY construction in-memory until
  Librarian validation prevents malformed TQUERYs from ever
  touching disk. The pending→resolved move via `archive_asset()`
  reuses the existing non-destructive infrastructure from v0.1.0
  rather than inventing a new file operation.

## 3. Evaluated Alternatives
### Option A — In-memory construction + Librarian validation (Selected)
* **Pros:** Reuses v0.1.0 infrastructure (Librarian, archive_asset,
  atomic writes). No malformed TQUERYs on disk.
* **Cons:** None significant — straightforward extension of
  existing patterns.

### Option B — Direct file write by originating agent
* **Pros:** Simpler — one less hop.
* **Cons:** Violates BREQ_010/012 — Librarian must be exclusive
  writer. Risk of schema drift across agents.

## 4. AREQ Boundary Compliance
* All file operations via FilesystemAdapter per AREQ_002.
* TQUERY presentation format fits within TDR_003 active artifact
  budget (2,000 tokens) — long TQUERYs truncate the
  `problem_statement` with a pointer to the full file.

## 5. Architect Validation
* **Validation required:** NO — within ADR_004 open space,
  reuses established patterns.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_013 (TQUERY dataclass & validation),
  TREQ_014 (presentation & resolution flow)
* **DEV_TICKETs impacted:** TQUERY pipeline implementation
