---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_015
type: BDR
title: "Repository Structure & Core Governance"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_015
    parent_id: null
  horizontal:
    depends_on: []
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: "BDR_010"
      link_reason: "Restructure governance plane to enforce
        semantic layer isolation — BDRs and BREQs consolidated
        under /governance/business/ as a unified business layer,
        replacing the previous flat sibling folder structure."

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Business Decision Record: BDR_015 — Repository Structure & Core Governance

## 1. Status & Metadata
* **BDR ID:** BDR_015
* **Date/Timestamp:** 2026-06-03 UTC
* **Type:** Core Infrastructure Policy — Supersedes BDR_010
* **Impacted Scope Tracks:** All Agentic Governance Lifecycle Documents

## 2. Context & Problem Statement
BDR_010 established the initial governance plane structure but placed BDRs and BREQs as independent sibling folders under `/governance/`. This structure fails to reflect their shared semantic identity as business layer artifacts, creating ambiguity about which folder layer an artifact belongs to and violating the principle of semantic cohesion established in BDR_011.

## 3. The Business Decision (The "What")
The repository root is strictly divided into two distinct logical planes: Execution (Development) and Governance (Specification).

### 3.1 The Governance Plane (`/governance/`)
All automated lifecycle agents operate exclusively within this directory tree. The Librarian enforces this sub-tree structure organized by semantic layer:

* **`/governance/business/`** — The unified business layer. Contains all strategic direction records (BDRs) and business requirements (BREQs), organized in dedicated sub-folders:
    * `/governance/business/BDRs/` — Business Decision Records. Strategic mandates and constitutional rules.
    * `/governance/business/BREQs/` — Business Requirements. Operational rules and agent mandates derived from BDRs.
    * Business baseline artifacts (e.g., `01_business_request.md`) live directly in `/governance/business/`.

* **`/governance/product/`** — Product layer. Contains Product Decision Records (PDRs) and Product Requirements (PREQs), organized in dedicated sub-folders:
    * `/governance/product/PDRs/`
    * `/governance/product/PREQs/`

* **`/governance/architecture/`** — Architecture layer. Contains Architectural Decision Records (ADRs), Architectural Requirements (AREQs), and Technical Determination Records (TDRs), organized in dedicated sub-folders:
    * `/governance/architecture/ADRs/`
    * `/governance/architecture/AREQs/`
    * `/governance/architecture/TDRs/`

* **`/governance/technical/`** — Technical layer. Contains Technical Requirements (TREQs), data schemas, and API contracts, organized in dedicated sub-folders:
    * `/governance/technical/TREQs/`
    * `/governance/technical/schemas/`

* **`/governance/templates/`** — Centralized master YAML schemas and clean markdown scaffolding forms.

* **`/governance/bprops/`** — Active engine history logs, metadata tokens, and operational snapshots.
    * `/governance/bprops/history/` — Global audit ledgers and change provenance chains.
    * `/governance/bprops/rollbacks/` — Immutable snapshots of rejected or failed structural states.

### 3.2 The Development Plane (`/src/`)
Reserved entirely for codebase logic, application source files, components, and technical unit tests. Governance agents do not alter this directory without explicit, human-directed deployment triggers.

## 4. Strategic Rationale & Consequences (The "Why")
* **Semantic Cohesion:** BDRs and BREQs share the same decision layer — business intent. Co-locating them under `/governance/business/` makes their relationship explicit and structurally enforced.
* **Layer Clarity:** Every artifact's location in the filesystem now directly communicates which governance layer it belongs to, eliminating ambiguity for agents and human operators.
* **Consistent Pattern:** All four governance layers (business, product, architecture, technical) follow the same sub-folder pattern, making the structure predictable and extensible.
* **Downstream Alignment:** The Librarian must enforce this structure on every file write operation. Any artifact placed outside its designated layer folder must be rejected and flagged via TQUERY.
