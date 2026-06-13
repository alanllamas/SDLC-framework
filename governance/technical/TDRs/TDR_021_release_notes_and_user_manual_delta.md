---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_021
type: TDR
title: "Release Notes & User Manual Delta Generation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.11.0 — Agent 8: Product Delivery Translator"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_019, TDR_020, TDR_008]
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

# Technical Decision Record: TDR_021 — Release Notes & User Manual Delta Generation

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** What Agent 8 produces from chronicle
  entries — release notes and user manual content — and where
  these are stored. Completes GAP_007's Agent 8 portion.

## 2. The Implementation Decision
* **Selected Approach:** Two output types, both reusing TDR_008
  sign-off:

  1. **Release Notes** — one file per milestone,
     `docs/release_notes/RELEASE_NOTES_[milestone].md`. Agent 8
     groups all unconsumed chronicle entries sharing a
     `version_tag.milestone`, translates their technical
     summaries into plain-language "what's new" bullets grouped
     by DEV_TICKET type (FEATURE/BUG/DEBT from TREQ_034).

  2. **User Manual Delta** — every DEV_TICKET carries a
     "Documentation Assignment" placeholder (established since
     v0.1.0 DEV_001's "Installation & Setup" etc.). Agent 8
     reads these placeholders from tickets corresponding to
     consumed chronicles and writes/updates the matching section
     in `docs/user_manual/[section_slug].md` — slug derived from
     the placeholder text (e.g., "Installation & Setup" →
     `installation_and_setup.md`).

  On sign-off of either output, Agent 8 sets
  `consumed_by_agent_8: true` on all chronicle entries that fed
  into it.

* **Rationale:** The "Documentation Assignment" field has existed
  on every DEV_TICKET since v0.1.0 specifically to seed this
  step — Agent 8's job is mechanical translation + routing to
  the placeholder already named by the Tech Lead at ticket
  creation, not invention of documentation structure from
  scratch. Grouping release notes by milestone aligns directly
  with PRD_001's milestone structure — "what shipped in v0.9.0"
  is a natural release notes unit.

## 3. Evaluated Alternatives
### Option A — Release notes per milestone + user manual delta via existing placeholders (Selected)
* **Pros:** Placeholders already exist from every prior
  milestone's DEV_TICKETs — no retrofitting. Release notes
  align with PRD_001 milestone structure.
* **Cons:** None significant.

### Option B — Single combined "what's new" document, no user manual structure
* **Pros:** Simpler.
* **Cons:** Discards the Documentation Assignment structure
  already present on 35+ DEV_TICKETs since v0.1.0 — wasted
  existing metadata.

## 4. AREQ Boundary Compliance
* All writes via FilesystemAdapter, atomic per TREQ_007,
  sign-off via TDR_008.

## 5. Architect Validation
* **Validation required:** NO — consumes existing metadata
  (Documentation Assignment, version_tag.milestone) per
  established schemas.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_038 (release notes & user manual
  delta generation, consumed flag update)
* **DEV_TICKETs impacted:** Agent 8 output generation
* **Resolves:** GAP_007 fully (Agents 7 and 8 both operational)
