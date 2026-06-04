---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_014
type: BDR
title: "Multi-Project Registry & Cross-Intelligence Protocol"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_014
    parent_id: null
  horizontal:
    depends_on: [BDR_001, BDR_006]
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

# Business Decision Record: BDR_014 — Multi-Project Registry & Cross-Intelligence Protocol

## 1. Status & Metadata
* **BDR ID:** BDR_014
* **Date/Timestamp:** 2026-06-03 UTC
* **Type:** Operational Configuration Policy
* **Impacted Scope Tracks:** Governs all multi-project interactions; extends BDR_001 and BDR_006

## 2. Context & Problem Statement
The framework is designed to govern individual projects in isolated repositories. However, operators running multiple projects simultaneously risk scope overlap, feature duplication, and architectural conflicts between projects without a mechanism to detect and surface these issues. The framework must define how projects remain isolated while enabling controlled cross-project intelligence to prevent operators from inadvertently working against themselves.

## 3. The Business Decision (The "What")

### 3.1 Project Isolation Principle
Each project governed by the framework must reside in its own dedicated repository with its own complete and independent `/governance/` plane. No governance artifacts from one project may be referenced as active dependencies in another project's governance plane. Projects are structurally sovereign units.

### 3.2 The Project Registry
A Project Registry is hereby established as the single source of truth for all active projects under the operator's governance ecosystem:
* **Scope:** The registry is a lightweight index — it does not replicate governance artifacts. It only captures each project's identity, sealed scope summary, and repository reference.
* **Location:** The registry lives outside any individual project repository in a dedicated operator-level governance space.
* **Mandatory Fields per Project Entry:**
    * Project ID and name
    * Repository reference
    * Sealed scope summary (derived from its active BDRs)
    * Current lifecycle status (Active, Paused, Archived)
* **Custodianship:** The registry is read-accessible to all agents across all projects. Write access is exclusively held by the Librarian of the project being registered, authorized by the HITL.

### 3.3 Cross-Intelligence Protocol
Agents do not autonomously monitor other projects. Instead, cross-project intelligence is activated naturally through the existing escalation process:
* When any agent detects that a feature, requirement, or architectural decision may overlap with or belong to another project, it consults the Project Registry as read-only context before issuing its structured conflict or escalation report to the HITL.
* The HITL receives the enriched report with explicit references to the potentially affected project and makes the final routing decision.
* No agent may autonomously move, copy, or reference artifacts between project repositories without explicit HITL authorization.

### 3.4 Anti-Overlap Mandate
The Business Catalyst of any active project is explicitly mandated to consult the Project Registry during the initial scope definition phase to verify that the incoming project's scope does not duplicate or directly conflict with an existing active project's sealed scope.

## 4. Strategic Rationale & Consequences (The "Why")
* **Project Sovereignty:** Each project remains a clean, self-contained governance unit with no implicit dependencies on other projects.
* **Operator Clarity:** The Project Registry gives the operator a single place to understand their full development ecosystem without navigating multiple repositories.
* **Conflict Prevention:** By consulting the registry at scope definition time, the framework catches overlap before development begins rather than after resources have been invested.
* **Scalability:** The protocol scales naturally — adding a new project requires only a registry entry, not changes to existing project governance planes.
* **Process Reuse:** The cross-intelligence mechanism reuses the existing escalation and conflict loops already defined in BREQ_006, requiring no new agent behavior — only new context.
