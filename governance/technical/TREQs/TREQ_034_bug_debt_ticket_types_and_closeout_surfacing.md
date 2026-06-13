---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_034
type: TREQ
title: "BUG/DEBT Ticket Types & Batch Closeout Surfacing"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.9.0 — Phase 2 Execution: Developer Workflow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_017
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

# Technical Requirement: TREQ_034 — BUG/DEBT Ticket Types & Closeout Surfacing

## 1. Intent & Scope
Adds `BUG` and `DEBT` as DEV_TICKET type values and defines how
`PROPOSED` tickets of these types surface during Batch Closeout
(TDR_014).

## 2. Technical Constraints

### Ticket Type Extension
* DEV_TICKET front-matter `type` field (new, added this milestone)
  accepts: `FEATURE` (default for existing tickets — implicit),
  `BUG`, `DEBT`.
* `BUG` tickets: `status: PROPOSED` until triaged. Carry
  `depends_on: [originating_ticket_id]` for context.
* `DEBT` tickets: `status: PROPOSED`,
  `version_tag.milestone: BACKLOG` — explicitly NOT assigned to
  a numbered milestone until triaged.

### Closeout Surfacing
* `generate_icd()` (TREQ_027) gains an additional check for
  Technical layer closeouts: scan for `status: PROPOSED` tickets
  of type `BUG` or `DEBT`.
* If any exist, ICD includes a non-blocking informational section:
```
PROPOSED tickets pending triage:
- [BUG_ID] {title} (blocks: {depends_on})
- [DEBT_ID] {title} (BACKLOG)
```
* This does NOT affect `overall_status` (informational only) —
  HITL triages during the closeout review by either:
  - Promoting to `status: AUTHORIZED` with a `version_tag.milestone`
    (moves into normal ticket flow per TREQ_031), or
  - Leaving as `PROPOSED`/`BACKLOG` for a future closeout.

## 3. Verification & QA Criteria
* DEV_TICKET schema accepts `type: BUG` and `type: DEBT` values.
* BUG tickets created via TREQ_033 choice B carry correct
  `depends_on`.
* DEBT tickets created via TREQ_033 choice C carry
  `version_tag.milestone: BACKLOG`.
* ICD includes PROPOSED ticket section when any exist —
  informational, does not change overall_status.
* Promoting a PROPOSED ticket to AUTHORIZED with a milestone
  makes it appear in `list_available_tickets()` (TREQ_031).
