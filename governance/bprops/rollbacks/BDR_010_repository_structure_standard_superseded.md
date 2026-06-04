---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_010
type: BDR
title: "Repository Structure & Core Governance"
status: REVIEW
relationships:
  vertical:
    governing_bdr: BDR_010
    parent_id: null
  horizontal:
    depends_on: []
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: "BDR_010_stale"
      link_reason: "Promote the templates folder to a top-level directory under /governance/ for direct visibility, housing master schemas and scaffolding forms."

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T06:00:00Z" }
  hitl_signatory: null
---

# BDR_010: Repository Structure & Core Governance

## 1. Objective
To establish a strict architectural boundary between the system's governance metadata and the production application code, ensuring modularity, data isolation, and security perimeter enforcement across all operations.

## 2. Root-Level Architecture
The repository root is strictly divided into two distinct logical planes: Execution (Development) and Governance (Specification).

### 2.1 The Governance Plane (`/governance/`)
All automated lifecycle agents operate exclusively within this directory tree. The Librarian enforces this sub-tree structure:
* **`/governance/BDRs/`** — Strategic direction records, foundational mandates, and constitutional rules.
* **`/governance/templates/`** — Centralized master YAML schemas and clean markdown scaffolding forms.
* **`/governance/business/`** — High-level business requirements, workflows, and translation layers.
* **`/governance/product/`** — Product requirements documents (PRDs), functional scopes, and user stories.
* **`/governance/architecture/`** — Architectural Decision Records (ADRs) and system blueprints.
* **`/governance/technical/`** — Technical constraints, data schemas, and API contracts.
* **`/governance/bprops/`** — Active engine history logs, metadata tokens, and operational snapshots.
    * `/governance/bprops/history/` — Global audit ledgers and change provenance chains.
    * `/governance/bprops/rollbacks/` — Immutable snapshots of rejected or failed structural states.

### 2.2 The Development Plane (`/src/`)
* Reserved entirely for codebase logic, application source files, components, and technical unit tests. Governance agents do not alter this directory without explicit, human-directed deployment triggers.

## 3. Root-Level Infrastructure & Isolation
The absolute root of the repository houses the foundational environment and tracking configurations required to protect the integrity of the workspace.

### 3.1 Master `.gitignore` Specification
The root `/.gitignore` acts as the master security boundary, isolating local configurations from the global repository history:

```gitignore
# Git Butler Local Environment Configuration
.butler.env

# Local Environment Overrides (Development Plane Safety)
.env*.local