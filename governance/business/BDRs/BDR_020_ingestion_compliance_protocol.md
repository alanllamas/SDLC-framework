---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_020
type: BDR
title: "Ingestion Compliance Protocol"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_020
    parent_id: null
  horizontal:
    depends_on: [BDR_001, BDR_005, BDR_006, BDR_018]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: "BDR_019"
      link_reason: "Add explicit Skip Declaration mandate to Coverage Check.
        Silence on skipped artifacts is never acceptable. Skips must be
        reviewed by all downstream layers and flagged by BDR_018 if unresolved."

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Business Decision Record: BDR_020 — Ingestion Compliance Protocol
## Supersedes BDR_019

## 1. The Transversal Mandate
Every agent that receives artifacts from an upstream layer must execute the
Ingestion Compliance Protocol before generating any artifact of its own.

## 2. Protocol Execution Sequence

### Step 1 — Completeness Check
All upstream artifacts must be in AUTHORIZED status. DRAFT or REVIEW artifacts
cannot be ingested. Failures trigger a TQUERY to the HITL.

### Step 2 — Coverage Check with Skip Declaration Mandate
Every upstream artifact must have a downstream descendant OR an explicit
Skip Declaration. Silence is never acceptable.

**Skip Reason Types:**
* GOVERNANCE_POLICY — internal system rules, no user-facing behavior.
* DEFERRED — addressed in future cycle. Must declare owner and target phase.
* COVERED_BY_SIBLING — covered by sibling artifact. Must reference ID.
* TQUERY_PENDING — blocked by unresolved TQUERY. Must reference ID.

**Skip Propagation Rule:**
Every Skip Declaration must be inherited by all downstream layers. Each
subsequent layer must CONFIRM or CHALLENGE the skip. Unresolved skips
reaching BDR_018 are flagged as descending orphan candidates.

### Step 3 — Conflict Pre-Scan
Scan for contradictions, scope overlaps, or anti-goal violations before
generating any artifact. Conflicts resolved via BREQ_006.

### Step 4 — Compliance Declaration
Appended to receiving layer index document using ICD_XXX.md template.
Requires HITL sign-off before work begins.

## 3. Blocking vs Non-Blocking Results
* PASSED — agent proceeds.
* PASSED WITH FLAGS — HITL acknowledges and authorizes.
* FAILED — agent blocked, TQUERY issued.

## 4. Relationship to BDR_018
BDR_020 catches local gaps at each transition. BDR_018 catches global gaps
at phase close. Yellow items and unresolved skips carry forward automatically.
