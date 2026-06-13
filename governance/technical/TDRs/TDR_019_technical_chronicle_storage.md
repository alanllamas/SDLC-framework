---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_019
type: TDR
title: "Technical Chronicle Storage & Structure"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.10.0 — Agent 7: Technical Process Chronicler"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_018, TDR_008]
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

# Technical Decision Record: TDR_019 — Technical Chronicle Storage & Structure

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** Where and how Agent 7's output (technical
  chronicle entries) is stored, and how it relates to DEV_TICKETs
  and feeds Agent 8 (v0.11.0) per GAP_007.

## 2. The Implementation Decision
* **Selected Approach:** One chronicle entry per completed
  DEV_TICKET, stored at
  `governance/technical/chronicle/CHRONICLE_[DEV_TICKET_ID].md`.
  Each entry follows a fixed structure: ticket reference, files
  touched (from code delta), technical summary (what changed and
  how — Agent 7's analysis), and a `consumed_by_agent_8: false`
  flag in front-matter.

  Chronicle entries are DRAFT → AUTHORIZED via the existing
  TDR_008 sign-off flow — operator reviews Agent 7's summary
  (with diff shown per TREQ_015) before sign-off. This keeps
  chronicle generation consistent with every other document
  type's review process — no special-cased "auto-accept"
  for agent-generated docs.

* **Rationale:** One file per ticket keeps chronicle entries
  small and reviewable (fits TDR_003 budget easily). The
  `consumed_by_agent_8` flag gives v0.11.0 a simple query —
  "which AUTHORIZED chronicle entries haven't been folded into
  release notes yet" — without Agent 8 needing to re-derive
  what's new.

## 3. Evaluated Alternatives
### Option A — One file per ticket + sign-off + consumed flag (Selected)
* **Pros:** Small reviewable files, reuses TDR_008 sign-off,
  simple Agent 8 query via flag.
* **Cons:** Many small files over time — acceptable, mirrors
  ticket-per-file pattern already used for DEV/TEST tickets.

### Option B — Single running changelog file, append-only
* **Pros:** One file to read for full history.
* **Cons:** Sign-off semantics unclear for append-only growing
  file (RULE_005 DRAFT/AUTHORIZED doesn't fit a perpetually-DRAFT
  file well), harder for Agent 8 to identify "what's new since
  last release".

## 4. AREQ Boundary Compliance
* All writes via FilesystemAdapter, atomic per TREQ_007, sign-off
  via TDR_008.

## 5. Architect Validation
* **Validation required:** NO — extends existing document
  lifecycle (TDR_008) to a new document type.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_036 covers generation; storage
  structure defined here informs TREQ_036's output format.
* **DEV_TICKETs impacted:** Agent 7 implementation
* **Sets up:** v0.11.0 Agent 8 consumption via
  `consumed_by_agent_8` flag
