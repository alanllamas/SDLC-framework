---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_017
type: BDR
title: "Non-Destructive Operations Mandate"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_001
    parent_id: BDR_001
  horizontal:
    depends_on: [BDR_003, BDR_008, BDR_015]
  history:
    supersedes_document:
      document_id: null
      link_reason: null
    amendment_note: >
      depends_on corrected 2026-06-03: BDR_010 (SUPERSEDED) replaced
      with BDR_015 (AUTHORIZED successor — Repository Structure &
      Core Governance). Finding surfaced by GIR_001 Plane 1.
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T00:00:00Z" }
  amendment_layer: { agent_identity: "Tech Lead / GIR_001 resolution", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BDR_017: Non-Destructive Operations Mandate

## 1. Core Principle
Every operation performed by any agent, adapter, or automated
process within the SDLC Governance Framework must be
non-destructive by default. No data, artifact, code, or state
may be permanently deleted, overwritten, or made unrecoverable
without explicit HITL confirmation.

## 2. Operative Rules
* **Archive, don't delete.** Artifacts replaced by supersession
  (per BDR_015) are moved to the rollbacks archive, never erased.
* **Atomic writes only.** All filesystem writes via Librarian use
  the temp-then-rename pattern (TREQ_007) — partial writes are
  never visible to other processes.
* **Supersession chain integrity.** Superseded artifacts remain
  addressable via their original ID and carry a
  `superseded_by` reference to their successor.
* **No silent overwrites.** Any write to an existing file that
  changes content must go through TDR_008 diff-presentation
  and HITL sign-off, except for append-only Ledger entries.
* **Branch retention.** Git Butler never force-pushes or deletes
  remote branches without explicit HITL instruction.

## 3. Skip Declaration Protocol
When an agent determines that a downstream artifact does not
require generation (governance policy, sibling coverage, or
HITL-authorized deferral), it must record an explicit Skip
Declaration — silence is a violation, not implicit approval.

## 4. Compliance Scope
Applies to: Librarian, Git Butler, all agent modules,
all adapter implementations. No exemptions.

## Amendment History
| Date | Change | Authorized by |
|:-----|:-------|:--------------|
| 2026-06-03 | depends_on: BDR_010 → BDR_015 (supersession correction per GIR_001 Plane 1 finding) | Alan Llamas |
