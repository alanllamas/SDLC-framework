---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_012
type: BDR
title: "HITL Operational Spectrum"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_012
    parent_id: null
  horizontal:
    depends_on: [BDR_005]
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

# Business Decision Record: BDR_012 — HITL Operational Spectrum

## 1. Status & Metadata
* **BDR ID:** BDR_012
* **Date/Timestamp:** 2026-06-03 UTC
* **Type:** Operational Configuration Policy
* **Impacted Scope Tracks:** Governs all HITL interaction mandates across BREQ_004 through BREQ_011

## 2. Context & Problem Statement
The framework was initially designed assuming a fully staffed human team occupying each HITL role. However, real-world deployment scenarios range from solo operators to incomplete teams, requiring the framework to remain operational and governable regardless of human availability at any given role.

## 3. The Business Decision (The "What")
The framework hereby recognizes four canonical operational configurations, each fully supported and governed by explicit rules:

* **Config 1 — Full Human Team:** Every HITL role is occupied by a distinct human operator. The framework operates as originally designed with no modifications.
* **Config 2 — Single Human Operator:** One human sequentially occupies all HITL roles. The framework must enforce mandatory cooling-off gates between role transitions to prevent self-approval conflicts.
* **Config 3 — Human + Delegated Agent HITLs:** The human operator retains strategic authority and explicitly delegates specific HITL roles to agent profiles on a per-session or permanent basis. Delegation and revocation are always human-initiated.
* **Config 4 — Incomplete Team with Temporary Agent HITL:** A human team missing one or more members temporarily assigns agent profiles to vacant roles. Agent authority is explicitly time-boxed and auditable, and reverts automatically upon human reinstatement.

## 4. Strategic Rationale & Consequences (The "Why")
* **Operational Resilience:** Prevents the framework from becoming inoperable due to team size or availability constraints.
* **Scope Flexibility:** Allows solo developers and small teams to adopt the framework without requiring a full organizational structure.
* **Governance Integrity:** Regardless of configuration, all decisions remain traceable, auditable, and reversible — the human sovereign authority is never structurally eliminated, only temporarily delegated.
* **Downstream Alignment:** All BREQs governing individual agent mandates must respect the active operational configuration declared at project initialization.
