---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PRD_XXX
type: PRD
title: "Product Roadmap: [Product Name]"
status: DRAFT
relationships:
  vertical:
    governing_bdr: BDR_002
    parent_id: null
  horizontal:
    depends_on: [BREQ_018]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "PM Orchestrator", timestamp: "[ISO-8601]" }
  hitl_signatory: null

# USAGE NOTE:
# Living document — never sealed, never marked AUTHORIZED.
# One PRD per product. Updated at every phase closeout per BREQ_003.
# Owned exclusively by PM Orchestrator via Librarian.
---

# Product Roadmap: [Product Name]

## 1. Vision Anchor
> Derived from Business Catalyst — maximum 3 sentences.

[The complete product vision]

---

## 2. Versioning Semantics
> Declared by Business Catalyst at first planning cycle.

| Version Type | Trigger | Planning Cycles Required |
|:-------------|:--------|:------------------------|
| **Major (X.0.0)** | New primary operator profile or paradigm shift | Business + Product + Arch + Tech |
| **Minor (X.Y.0)** | New significant capability within same primary operator | Product + Arch + Tech |
| **Patch (X.Y.Z)** | UX improvement, bug fix, performance — no new scope | Tech (with Arch review) |

**Custom semantics declaration:** [If modified, declare here with rationale]

---

## 3. Version Registry

### Version: [X.Y.Z] — [Version Name]
* **Primary User Profile:** [From Operator Profile Registry]
* **Secondary User Profiles:** [Or NONE]
* **Scope Boundary:**
    * **Does:** [What this version delivers]
    * **Does Not:** [What this version excludes]
* **Planning Status:** PLANNED | IN_PROGRESS | COMPLETE | DEFERRED
* **Dependencies:** [Versions required first or NONE]
* **Governing PDRs:** [List of PDR IDs]
* **Last Updated:** [ISO-8601]

#### Milestones
| Milestone ID | Name | Assigned PREQs | Status | MVP? |
|:-------------|:-----|:---------------|:-------|:-----|
| v0.1.0 | [Name] | [PREQ_XXX] | PLANNED \| IN_PROGRESS \| RELEASED | |

*(Mark exactly one milestone per major version with ⭐ MVP)*

---

## 4. Gap Registry
> All planning gaps. Resolved gaps marked in-place — never deleted.

| Gap ID | Description | Blocks Version | Owner Layer | Status |
|:-------|:------------|:--------------|:------------|:-------|
| GAP_001 | [Description] | [vX.Y.Z] | Business \| Product \| Architecture \| Technical | OPEN \| IN_PROGRESS \| RESOLVED |

---

## 5. Release History
> Updated by Git Butler when a milestone is tagged.

| Milestone | Release Date | Git Tag | Notes |
|:----------|:------------|:--------|:------|
| — | — | — | No releases yet |
