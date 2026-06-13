---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_020
type: TREQ
title: "CIR Presentation, Resolution, Escalation & Conflict History Query"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.6.0 — Conflict Intelligence & History"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_010
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

# Technical Requirement: TREQ_020 — CIR Presentation, Resolution, Escalation & History

## 1. Intent & Scope
Defines the three-tier presentation template, resolution
capture, escalation flow, and conflict history query per
PREQ_007 sections 2.1-2.4.

## 2. Technical Constraints

### Presentation Template
```
⚠️ CONFLICT [CIR_NNN] — {loop} ({layers_involved joined by " ↔ "})

THE CONFLICT:
{the_conflict}

AGENT PROPOSALS:
- [{proposal.agent}]: {proposal.proposal}
  (... one per agent_proposals entry)

YOUR DECISION:
{decision_slot_prompt}

This blocks {active_branch} until resolved.
```
* Agent proposals explicitly labeled — never presented as
  decisions, per PREQ_007 section 2.1.

### Resolution Capture
* Operator selects a proposal, provides custom direction, or
  requests escalation.
* `Librarian.resolve_cir(cir_id, resolution)`:
    1. Appends resolution block (selected proposal or custom
       text + acting agent + artifact impact).
    2. `archive_asset()` move pending/ → resolved/.
    3. Removes CIR_ID from `state.yaml.session.pending_cirs`.
    4. Generates Ledger entry per BREQ_013.
    5. Unfreezes `active_branch`.

### Escalation Flow
* If operator requests escalation: CIR's `loop` field updated
  to next loop up (LOOP_3→LOOP_2→LOOP_1), full payload carried
  forward, re-presented to the higher-loop agent context.
* Escalation logged in Ledger as `CIR_ESCALATED` event.

### Conflict History Query
* `Librarian.query_cir_history(project_id)` returns all
  resolved + pending CIRs as scannable summary:
  CIR_ID, loop, layers, status, resolution (if resolved).
* No detail expansion unless operator requests specific CIR_ID.

### Retrofit of v0.5.0 TREQ_018 Conflict Path
* `detect_conflict()` (TREQ_018) now calls `Librarian.write_cir()`
  instead of generating an interim TQUERY, using LOOP_2
  (Product ↔ Architecture constraint conflicts).

## 3. Verification & QA Criteria
* CIR rendered with exactly THE CONFLICT / AGENT PROPOSALS /
  YOUR DECISION sections.
* Resolution captures selected proposal or custom text +
  consequence before execution.
* Escalation updates loop field, carries full payload, logs
  CIR_ESCALATED.
* History query returns scannable summary without full detail
  unless requested.
* TREQ_018 constraint-conflict path produces a CIR, not a TQUERY.
