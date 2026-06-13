---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_018
type: BREQ
title: "Product Roadmap Document Standard"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_002
    parent_id: null
  horizontal:
    depends_on: [BREQ_003, BREQ_004]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BREQ_018: Product Roadmap Document Standard

## 1. Core Objective
Establish the Product Roadmap Document (PRD) as a mandatory living
artifact capturing the complete versioned evolution of the product
— from MVP to full business vision realization.

## 2. The PRD vs Other Product Artifacts
* **PDR:** Single product decision — immutable once authorized.
* **PREQ:** Single user-facing behavior — scoped to a version.
* **PRD:** Living map of all planned versions — evolves via
  controlled operations managed exclusively by the Librarian.

## 3. Mandatory PRD Content Structure

### 3.1 Vision Anchor
Maximum 3-sentence statement of complete product vision.

### 3.2 Versioning Semantics Declaration
Business Catalyst declares versioning semantics. Default pattern:

| Version Type | Trigger | Planning Cycles Required |
|:-------------|:--------|:------------------------|
| **Major (X.0.0)** | New primary operator profile or paradigm shift. Anti-Goal modified. | Business + Product + Arch + Tech |
| **Minor (X.Y.0)** | New capability within same primary operator. New HITL config, agent layer, or integration. | Product + Arch + Tech |
| **Patch (X.Y.Z)** | UX improvement, bug fix, performance — no new scope. | Tech (with Arch review) |

**Redefinition Rule:** If default doesn't fit, Business Catalyst
declares alternative in project's first BDR cycle. Framework
validates semantics exist — not that they follow default.

### 3.3 Version Registry
Each Version Entry contains:
* Version ID, Version Name
* Primary/Secondary User Profile (from Operator Profile Registry)
* Scope Boundary (Does/Does Not)
* Planning Status: PLANNED | IN_PROGRESS | COMPLETE | DEFERRED
* Planning Gaps, Dependencies
* Last Updated timestamp
* Governing PDRs
* **MVP declaration:** Which milestone constitutes the MVP —
  the minimum demonstrable point where core value proposition
  is verifiable. MVP is not the complete version.

### 3.4 PDR Change Trigger
When a referenced PDR is superseded, PM Orchestrator must:
1. Review impact on affected Version Entry.
2. Update scope boundary and governing PDRs list.
3. Flag resulting Planning Gaps.
4. Log update in Global History Ledger via Librarian.

### 3.5 Gap Registry
Lives within the PRD as a section — not a separate file.
* Gap ID, Description, Blocks Version, Owner Layer, Status
* Resolved gaps marked in-place — never deleted.

## 4. PRD Lifecycle Rules

### 4.1 Document Operations
* New Version Entries: appended to Version Registry.
* Updates to existing entries: edited in-place with last_updated
  timestamp — not immutable, PRD is living document.
* New gaps: appended with sequential ID.
* Resolved gaps: marked RESOLVED in-place.

### 4.2 Ownership & Updates
* One PRD per product.
* PM Orchestrator exclusive editorial authority.
* Initialized at start of first planning cycle.
* Updated at every Batch Closeout per BREQ_003.

### 4.3 Milestone Tagging
* Each Version Entry includes Milestones table:
  Milestone ID, Name, Assigned PREQs, Status (PLANNED|IN_PROGRESS|RELEASED)
* Artifacts tagged via version_tag in master_metadata.yaml:
```yaml
version_tag:
  target_version: "string (OPTIONAL)"
  milestone: "string (OPTIONAL)"
```
* Milestone Status Updates: Git Butler tags repo when milestone
  released. PM updates milestone status to RELEASED in PRD.

## 5. Business Compliance & QA Metrics
* **Metric 1:** One active PRD per product once first cycle begins.
* **Metric 2:** Versioning semantics declared before first Version Entry.
* **Metric 3:** Every version has declared primary user profile.
* **Metric 4:** 100% of planning gaps explicitly declared.
* **Metric 5:** PRD updated at every phase closeout.
* **Metric 6:** Every PDR supersession triggers PRD review.
* **Metric 7:** Every Version Entry declares its MVP milestone.
