---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_016
type: BREQ
title: "Multi-Project Registry & Cross-Intelligence Operational Protocol"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_014
    parent_id: null
  horizontal:
    depends_on: [BREQ_001, BREQ_003, BREQ_006]
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

# BREQ_016: Multi-Project Registry & Cross-Intelligence Operational Protocol

## 1. Core Objective
To define the operational rules for maintaining the Project Registry, the mandatory consultation protocol for the Business Catalyst at scope initialization, and the cross-intelligence read protocol for all agents during escalation and conflict resolution events.

## 2. Project Registry Structure & Maintenance

### 2.1 Registry Entry Requirements
Every project registered in the ecosystem must maintain the following fields in the Project Registry:
* **Project ID:** Unique identifier following the convention `PRJ_XXX`.
* **Project Name:** Human-readable name.
* **Repository Reference:** Full URL or path to the project's version-controlled repository.
* **Sealed Scope Summary:** A maximum 5-sentence plain-language description of what the project does and explicitly does not do, derived directly from its active BDRs.
* **Lifecycle Status:** One of the following states:
    * `ACTIVE` — Currently in development.
    * `PAUSED` — Temporarily halted, scope preserved.
    * `ARCHIVED` — Completed or permanently halted.
* **Last Updated Timestamp:** ISO-8601 timestamp of the last registry entry modification.

### 2.2 Registry Custodianship
* The Project Registry is written exclusively by the Librarian of the project being registered or updated, authorized by the HITL.
* All agents across all projects have read-only access to the full Project Registry.
* Any modification to a project's registry entry must be logged in that project's Global History Ledger with the authorizing HITL identity.

### 2.3 Registry Location
* The physical location and hosting mechanism of the Project Registry is deliberately left undefined at the business layer and must be resolved via an ADR before implementation.

## 3. Business Catalyst Scope Initialization Protocol

### 3.1 Mandatory Registry Consultation
Before the Business Catalyst may accept or formalize any new project scope, it must execute the following sequence:
1. **Registry Read:** Load the full Project Registry and identify all projects in `ACTIVE` or `PAUSED` status.
2. **Scope Overlap Scan:** Compare the incoming project's proposed scope against the sealed scope summaries of all active and paused projects.
3. **Conflict Detection:** If a potential overlap or direct conflict is detected, the Business Catalyst must halt scope formalization and issue a structured TQUERY to the HITL presenting:
    * The incoming project's proposed scope.
    * The existing project's sealed scope summary.
    * The specific overlap or conflict detected.
    * Actionable options for the HITL to resolve.
4. **Clear Passage:** If no overlap is detected, the Business Catalyst proceeds with scope formalization and registers the new project entry in the registry upon HITL authorization.

### 3.2 Scope Modification Trigger
If an existing project's scope is modified after registration, the Business Catalyst must re-execute the full registry consultation sequence to verify the modified scope does not create new overlaps with other active projects.

## 4. Cross-Intelligence Protocol for All Agents

### 4.1 Registry Consultation During Escalation
When any agent detects that a feature, requirement, or architectural decision may belong to or conflict with another project, it must:
1. Consult the Project Registry as read-only context.
2. Enrich its escalation or conflict report with explicit references to the potentially affected project entry from the registry.
3. Present the enriched report to the HITL for final routing decision.

### 4.2 No Autonomous Cross-Project Actions
* No agent may autonomously move, copy, reference, or link governance artifacts between project repositories under any circumstance.
* All cross-project actions require explicit HITL authorization and are logged in the Global History Ledger of both affected projects.

### 4.3 Registry Staleness Protocol
* If an agent consults the registry and detects that a project entry has not been updated in more than 30 days while in `ACTIVE` status, it must surface a staleness warning to the HITL before proceeding with its consultation.
* The HITL may dismiss the warning or trigger a registry update. Either action is logged.

## 5. Business Compliance & QA Metrics
* **Metric 1 (Registry Consultation Coverage):** 100% of new project scope formalizations must be preceded by a documented registry consultation. Zero unscanned project initializations permitted.
* **Metric 2 (Overlap Detection Response):** 100% of detected scope overlaps must trigger a structured TQUERY to the HITL before scope formalization proceeds.
* **Metric 3 (Cross-Project Action Authorization):** 0% of cross-project artifact interactions may occur without explicit HITL authorization and dual-project audit log entries.
* **Metric 4 (Registry Entry Completeness):** 100% of registered projects must maintain all required fields with zero null values in active or paused status.
* **Metric 5 (Staleness Flag Coverage):** 100% of registry entries exceeding 30 days without update in ACTIVE status must trigger a staleness warning upon next agent consultation.
