---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_014
type: BREQ
title: "HITL Operational Spectrum — Configuration Rules & Transition Protocol"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_012
    parent_id: null
  horizontal:
    depends_on: [BREQ_004, BREQ_007, BREQ_008, BREQ_010, BREQ_011]
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

# BREQ_014: HITL Operational Spectrum — Configuration Rules & Transition Protocol

## 1. Core Objective
To define the operational rules, activation conditions, and transition protocols for each of the four canonical HITL configurations established in BDR_012, ensuring the framework remains governable and auditable regardless of team size or human availability.

## 2. Configuration Activation Rules

### Config 1 — Full Human Team
* **Activation:** Declared at project initialization by the HITL. Requires at least one registered human identity per active agent role.
* **Operational Rule:** No agent may occupy a HITL seat. All sign-off events require a distinct human signature per role. No cooling-off gates apply.

### Config 2 — Single Human Operator
* **Activation:** Declared at project initialization or triggered by an explicit operator declaration mid-project.
* **The Cooling-Off Gate:** When a single operator transitions between HITL roles within the same session, the framework must enforce a mandatory cooling-off checkpoint:
    * The agent presents a structured summary of all decisions made in the previous role before the operator assumes the next role.
    * The operator must explicitly acknowledge the summary and confirm the role transition before proceeding.
    * This gate exists to prevent unconscious self-approval and force deliberate context switching.
* **Audit Flag:** All sign-off events in Config 2 are permanently flagged in the Global History Ledger as single-operator decisions for future audit transparency.

### Config 3 — Human + Delegated Agent HITLs
* **Activation:** Triggered by an explicit human delegation declaration specifying the target role, the agent profile, and the capability level as defined in BDR_013.
* **Operational Rule:** The human operator retains at minimum one HITL seat at all times. Full delegation of all roles simultaneously is prohibited in Config 3.
* **Revocation:** The human operator may revoke any delegated role at any point. Upon revocation the agent must immediately halt any in-progress drafting, preserve its current in-memory state, and transfer context to the human operator before standing down.

### Config 4 — Incomplete Team with Temporary Agent HITL
* **Activation:** Triggered when a registered human operator becomes unavailable and the team explicitly declares a vacancy rather than leaving the role silently unmanned.
* **Time-Boxing:** All Config 4 agent HITL assignments must carry an explicit expiration condition — either a calendar date or a project milestone — after which the role automatically reverts to pending human assignment.
* **Authority Ceiling:** Config 4 agent HITLs may not authorize BDR-level decisions. BDR authorization is reserved exclusively for human operators regardless of configuration.
* **Review Mandate:** All decisions made by a Config 4 agent HITL must be presented to the reinstated human operator in a structured review session before the project advances to the next phase gate.

## 3. Configuration Transition Protocol
* **Mid-Project Transitions:** A project may transition between configurations at any point with explicit HITL declaration. The transition event is logged in the Global History Ledger with the declaring operator's identity and timestamp.
* **No Silent Transitions:** The system must never autonomously shift between configurations. Configuration state is always human-declared.
* **Downgrade Safety:** Transitioning from a higher autonomy config to a lower one (e.g., Config 3 to Config 1) is always permitted instantly. Transitioning to a higher autonomy config requires explicit declaration and audit log entry.
* **Config Restore:** If an operator downgrades temporarily and declares intent to restore a previously authorized configuration, the system treats the restoration as a single-step reinstatement rather than a full escalation. The prior configuration record in the Global History Ledger serves as the authorization anchor. An audit log entry is still required.

## 4. Business Compliance & QA Metrics
* **Metric 1 (Configuration Declaration Coverage):** 100% of active projects must have an explicitly declared operational configuration at initialization. Zero undeclared configurations permitted.
* **Metric 2 (Cooling-Off Gate Compliance):** 100% of role transitions in Config 2 must pass through the cooling-off checkpoint. Zero silent role switches permitted.
* **Metric 3 (Authority Ceiling Enforcement):** 0% of BDR-level authorizations may carry a non-human signatory under any configuration.
* **Metric 4 (Transition Audit Coverage):** 100% of configuration transition events must appear in the Global History Ledger with operator identity and timestamp.
