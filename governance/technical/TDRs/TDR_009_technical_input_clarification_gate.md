---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_009
type: TDR
title: "Technical Input Interception & Clarification Gate Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.5.0 — Technical Input & Scope Clarification"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_006, TDR_008]
  cross_plane:
    architecture_adr: ADR_003
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_009 — Technical Input Interception & Clarification Gate

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_003 — Agent Orchestration Model
* **Decision Scope:** How agents detect technology mentions
  requiring clarification, how the fixed-option gate is rendered,
  and how routing to Vision Ledger vs hard constraints is
  implemented per BREQ_001 and PREQ_009.

## 2. The Implementation Decision
* **Selected Approach:** Detection of "this needs clarification"
  is an LLM reasoning task, not a separate pattern-matcher —
  each agent's system prompt (TREQ_011) includes BREQ_001's
  necessity-keyword guidance, instructing the agent to emit a
  **structured clarification directive** when it identifies an
  ambiguous technology mention. The Orchestrator recognizes this
  directive in the agent's response and renders it as a
  fixed-option gate (A/B only, no free text per PREQ_009 section 2.2)
  — overriding the agent's normal free-form response for that turn.

  Routing outcomes are deterministic Librarian operations:
  - **Hard Constraint** → appended to a structured "Active
    Constraints" list maintained in `01_business_request.md`.
  - **Preference** → appended to the existing "Operator Vision &
    Desires Feedback Ledger" section with a `PREFERENCE` tag.

  Anti-goal scan and scope conflict detection run as deterministic
  checks (substring/structured match against declared Anti-Goals
  and Active Constraints) before either routing completes.

* **Rationale:** Keeping detection as agent reasoning (not a
  separate NLP layer) avoids duplicating logic the LLM already
  does naturally per its system prompt — consistent with
  ADR_001's "no parallel implementations of agent intelligence."
  The structured directive gives the Orchestrator a deterministic
  hook to enforce the fixed-option UI constraint that pure LLM
  output cannot guarantee.

## 3. Evaluated Alternatives
### Option A — LLM-emitted structured directive + deterministic routing (Selected)
* **Pros:** No duplicate detection logic, deterministic routing
  and conflict checks, fixed-option UI guaranteed by Orchestrator
  regardless of LLM phrasing.
* **Cons:** Requires agents to emit a recognizable directive
  format — mitigated by adding it to TREQ_011 prompt structure.

### Option B — Separate keyword-matching pre-processor
* **Pros:** Fully deterministic detection.
* **Cons:** Duplicates agent intelligence, brittle keyword lists,
  diverges from BREQ_001's nuanced constraint-vs-preference framing.

## 4. AREQ Boundary Compliance
* Vision Ledger and Active Constraints writes via FilesystemAdapter,
  atomic per TREQ_007.
* Anti-goal and conflict scans are pure computation — no adapter calls.

## 5. Architect Validation
* **Validation required:** NO — within ADR_003 open space.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_017 (clarification directive &
  fixed-option rendering), TREQ_018 (Vision Ledger, Active
  Constraints & conflict scan)
* **DEV_TICKETs impacted:** Clarification gate and constraint
  routing logic
