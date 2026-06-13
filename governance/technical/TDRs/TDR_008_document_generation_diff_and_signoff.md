---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_008
type: TDR
title: "Document Generation, Diff Presentation & Sign-off Flow"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.4.0 — Document Generation & Sign-off"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_004, TDR_006, TDR_007]
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

# Technical Decision Record: TDR_008 — Document Generation, Diff Presentation & Sign-off Flow

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How agents generate documents, how diffs
  against previous versions are presented, and how HITL sign-off
  transitions a document DRAFT → AUTHORIZED per PREQ_006 and
  ADR_004 section 3.3 document state transitions.
* **Boundaries Respected:** Librarian exclusive writer per
  BREQ_010. Atomic writes per TREQ_007. RULE_005 — DRAFT freely
  editable, AUTHORIZED immutable.

## 2. The Implementation Decision
* **Selected Approach:** When an agent produces or modifies a
  document, the Orchestrator presents the **full rendered content**
  to the operator. If the document already existed (modification
  of a DRAFT), a unified diff against the last-saved version is
  shown above the full content. Sign-off is a single explicit
  confirmation step that the Librarian executes as one atomic
  operation: write final content + update front-matter
  (`status: AUTHORIZED`, `hitl_signatory`, timestamp) + Ledger entry.

  For documents transitioning AUTHORIZED → DEPRECATED via
  supersession, the new document's `history.supersedes_document`
  is pre-populated by the originating agent — the Librarian
  validates the referenced document exists and is AUTHORIZED
  before accepting the new document's sign-off.

* **Rationale:** Showing full content + diff (when applicable)
  in one response satisfies PREQ_006 without requiring the
  operator to request a diff separately. Single atomic sign-off
  operation prevents partial states (e.g., content written but
  front-matter not updated).

## 3. Evaluated Alternatives
### Option A — Full content + diff in single response, atomic sign-off (Selected)
* **Pros:** Operator sees everything needed to decide in one
  response. Atomic sign-off prevents inconsistent state.
* **Cons:** Long documents produce long responses — mitigated
  by TDR_003 active artifact token budget (2,000 tokens);
  documents exceeding budget are chunked with explicit notice.

### Option B — Diff-only with separate "show full" command
* **Pros:** Shorter default responses.
* **Cons:** Extra round-trip for operator on every new document
  (no previous version to diff against) — worse default experience.

## 4. AREQ Boundary Compliance
* All writes via FilesystemAdapter, atomic per TREQ_007.
* Diff generation is pure computation — no adapter calls.
* Documents exceeding 2,000 token budget per TDR_003 trigger
  chunked presentation with explicit "continued" markers —
  reuses truncation pattern from TREQ_006.

## 5. Architect Validation
* **Validation required:** NO — within ADR_004 open space.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_015 (document rendering & diff),
  TREQ_016 (sign-off capture & atomic status transition)
* **DEV_TICKETs impacted:** Document presentation and sign-off logic
