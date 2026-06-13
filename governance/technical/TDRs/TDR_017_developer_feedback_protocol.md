---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_017
type: TDR
title: "Developer Feedback Protocol — Unified Entry Point"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.9.0 — Phase 2 Execution: Developer Workflow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_001
  horizontal:
    depends_on: [TDR_007, TDR_010, TDR_012, TDR_016]
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

# Technical Decision Record: TDR_017 — Developer Feedback Protocol

## 1. Decision Context
* **Parent AREQ:** AREQ_001 — Offline & Connectivity Resilience
  (referenced as closest parent — this is about not losing
  developer-discovered intelligence during disconnected work)
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** Resolves GAP_009 — a unified entry point
  during Execution Mode for the developer to capture bugs,
  technical debt, blockers, innovations, and informal
  observations without losing intelligence or interrupting flow.

## 2. The Implementation Decision
* **Selected Approach:** A single Orchestrator command available
  during Execution Mode — `/log` (or natural-language equivalent
  "I found something") — opens a lightweight classification
  prompt:

```
What did you find?
A) Blocker — I can't proceed without resolving this
B) Bug — something is broken (not blocking current ticket)
C) Technical Debt — works, but should be improved later
D) Idea — innovation or improvement worth considering
E) Just a note — capture for later, no action needed
```

  Routing per choice, all reusing existing pipelines:
  - **A (Blocker)** → TQUERY (TDR_007) — freezes current
    DEV_TICKET branch, escalates per BDR_007 Tier system.
  - **B (Bug)** → new DEV_TICKET with `type: BUG`,
    `status: PROPOSED`, linked via `depends_on` to the ticket
    being worked — does NOT freeze current work.
  - **C (Technical Debt)** → new DEV_TICKET with `type: DEBT`,
    `status: PROPOSED`, `version_tag.milestone: BACKLOG` —
    surfaced at next Batch Closeout (TDR_014) for triage.
  - **D (Idea)** → BPROP (TDR_012) — enters upstream reflow
    from Technical layer.
  - **E (Note)** → appended to
    `governance/technical/01_technical_baseline.md` Future
    Horizon Ledger — zero process overhead, purely informational.

* **Rationale:** Every routing destination already exists from
  v0.3.0-v0.7.0 — this milestone adds only the classification
  prompt and two new DEV_TICKET types (BUG, DEBT). No new
  pipelines, no duplicated infrastructure. The developer never
  needs to know which artifact type to create — the `/log`
  command and five-option prompt handle classification.

## 3. Evaluated Alternatives
### Option A — Single classification prompt routing to existing pipelines (Selected)
* **Pros:** Zero new pipeline infrastructure, developer doesn't
  need governance vocabulary, all five outcomes traceable via
  existing Ledger mechanisms.
* **Cons:** None significant.

### Option B — Separate developer-facing issue tracker
* **Pros:** Familiar to developers from other tools.
* **Cons:** Parallel system to governance artifacts — exactly
  the fragmentation BDR_011 (single responsibility) and this
  TDR's rationale avoid.

## 4. AREQ Boundary Compliance
* All routing via existing Librarian operations, atomic per TREQ_007.
* `/log` command available offline — classification and DEV_TICKET/
  Future-Horizon writes don't require LLM call for routing itself
  (only for content elaboration if developer requests it).

## 5. Architect Validation
* **Validation required:** NO — routes to existing ADR_004
  pipelines, two new DEV_TICKET type enum values only.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_033 (classification prompt & routing),
  TREQ_034 (BUG/DEBT ticket types & Batch Closeout surfacing)
* **DEV_TICKETs impacted:** Developer feedback capture
* **Resolves:** GAP_009 (Developer Feedback Protocol)
