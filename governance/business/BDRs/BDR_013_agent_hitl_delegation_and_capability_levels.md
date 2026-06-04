---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_013
type: BDR
title: "Agent HITL Delegation & Capability Levels"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_013
    parent_id: null
  horizontal:
    depends_on: [BDR_012]
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

# Business Decision Record: BDR_013 — Agent HITL Delegation & Capability Levels

## 1. Status & Metadata
* **BDR ID:** BDR_013
* **Date/Timestamp:** 2026-06-03 UTC
* **Type:** Operational Configuration Policy
* **Impacted Scope Tracks:** Extends BDR_012 Config 3 and Config 4; governs all delegated Agent HITL behavior

## 2. Context & Problem Statement
BDR_012 establishes that agent profiles can occupy HITL roles under specific configurations. However, not all delegated agent HITLs should operate with the same authority or intelligence level. A solo developer learning the framework needs different agent support than a senior architect temporarily absent from the team. The framework must define how agent capability is configured, bounded, and grown over time without compromising governance integrity.

## 3. The Business Decision (The "What")

### 3.1 The Capability Spectrum
Every Agent HITL operates within one of three capability levels, explicitly configured by the human operator at delegation time:

* **Level 1 — Training Wheels:** The agent mirrors every decision back to the human with full explanation before executing. Designed for operators learning the framework. The agent proposes, the human always confirms.
* **Level 2 — Copilot:** The agent executes low-risk decisions autonomously but surfaces medium and high-risk decisions to the human with a structured recommendation. Designed for experienced operators who want acceleration without losing control.
* **Level 3 — Autonomous Delegate:** The agent executes all decisions within its delegated role scope independently, logging every action to the audit trail. Reserved for vacant roles in Config 4 where no human is available. All Level 3 actions are flagged for human review at the next available session.

### 3.2 The Copilot Mode (Dual Function)
When operating at Level 1 or Level 2, the Agent HITL serves a dual function simultaneously:
* **Role Executor:** Fulfills the delegated HITL role within the framework process.
* **Intelligence Amplifier:** Proactively surfaces context, implications, and recommendations to elevate the human's decision quality — acting as training support for operators growing into their role.

### 3.3 Delegation & Revocation Rules
* Delegation is always explicitly declared by the human operator — never assumed or auto-assigned by the system.
* Capability level must be declared at delegation time.
* Revocation is immediate and always available to the human operator at any point, regardless of pipeline state.
* All delegation and revocation events are permanently logged in the Global History Ledger.

### 3.4 Adaptive Capability Assessment
The Agent HITL continuously monitors operator interaction patterns to build an evolving intelligence profile:
* **Observable Signals:** Decision confidence, frequency of clarification requests, consistency of choices against framework rules, and depth of operator-provided context.
* **Suggestion Protocol:** When the agent detects sustained patterns indicating the operator has outgrown their current capability level, it may issue a single structured suggestion to the operator proposing a level adjustment. The agent never applies a level change autonomously — the human operator always confirms or rejects the suggestion.
* **Downgrade Protection:** If an operator's patterns suggest they are struggling at their current level, the agent may similarly suggest a downgrade. This is never punitive — it is presented as an optimization for decision quality and governance integrity.
* **Profile Persistence:** The operator's interaction profile is stored in the Global History Ledger and carries forward across sessions, ensuring the agent does not reset its understanding of the operator at each new conversation. The physical persistence mechanism for operator profiles is deliberately left undefined at the business layer and must be resolved via an ADR before implementation.

### 3.5 Operator Identity & Pair Mode
The framework explicitly supports operator identity changes and collaborative pair sessions within the same HITL seat:

* **Operator Handoff:** Any human operator may declare an identity change at any point in a session. The incoming operator must explicitly identify themselves before the framework resumes. The outgoing operator's capability profile is preserved; the incoming operator's profile loads automatically if previously registered, or initializes at Level 1 if new to the system.
* **Pair Programming Mode:** Two human operators may explicitly declare a shared session, activating Pair Mode. In this mode:
    * Both operator identities are captured in all sign-off events with dual signatures.
    * The Agent HITL adapts its capability level to the lower of the two operators' registered levels to ensure neither operator is left behind.
    * Either operator may independently trigger a TQUERY or escalation at any time.
* **Audit Trail Integrity:** All operator identity changes, pair mode activations, and deactivations are permanently logged as discrete events in the Global History Ledger with exact timestamps.

## 4. Strategic Rationale & Consequences (The "Why")
* **Accessibility:** Lowers the barrier of entry for solo operators and small teams adopting the framework without sacrificing governance quality.
* **Human Sovereignty:** The human operator always retains the ability to reclaim any delegated role instantly.
* **Auditability:** Every agent HITL action at any capability level is traceable, reviewable, and reversible.
* **Growth Path:** The capability spectrum creates a natural learning curve — operators start at Level 1 and graduate to Level 2 as they internalize the framework, without ever being forced to operate beyond their comfort zone.
* **Team Resilience:** Pair Mode and Operator Handoff ensure the framework adapts to real-world team dynamics without losing governance continuity.
* **Physical persistence mechanism:** The physical persistence mechanism for operator profiles is deliberately left undefined at the business layer and must be resolved via an ADR before implementation.
