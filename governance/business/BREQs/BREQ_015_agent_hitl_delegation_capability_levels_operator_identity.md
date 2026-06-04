---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_015
type: BREQ
title: "Agent HITL Delegation, Capability Levels & Operator Identity Protocol"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_013
    parent_id: null
  horizontal:
    depends_on: [BREQ_014]
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

# BREQ_015: Agent HITL Delegation, Capability Levels & Operator Identity Protocol

## 1. Core Objective
To define the operational execution rules for agent HITL delegation, capability level configuration, adaptive assessment behavior, and operator identity management as established in BDR_013.

## 2. Delegation Protocol

### 2.1 Delegation Declaration
* The human operator must explicitly declare the following at delegation time:
    * Target HITL role being delegated
    * Agent profile assigned to the role
    * Capability level (1, 2, or 3) as defined in BDR_013
    * Expiration condition for Config 4 assignments
* Delegation declarations are immediately logged in the Global History Ledger before the agent assumes the role.
* An agent may not begin operating in a delegated HITL role until the Librarian confirms the delegation entry has been written to the ledger.

### 2.2 Revocation Protocol
* Revocation is always human-initiated and takes effect immediately upon declaration.
* Upon revocation the agent must:
    1. Halt any in-progress drafting immediately.
    2. Compile a structured handoff summary of all in-progress work and pending decisions.
    3. Deliver the handoff summary to the human operator.
    4. Stand down and release the HITL seat.
* The revocation event and handoff summary reference are logged in the Global History Ledger.

## 3. Capability Level Execution Rules

### Level 1 — Training Wheels
* The agent must present a full explanation of every decision before requesting human confirmation.
* No action may be executed without explicit human confirmation per decision.
* The agent must surface at least one alternative path per decision to ensure the human is making an informed choice rather than rubber-stamping.

### Level 2 — Copilot
* The agent classifies every incoming decision as low-risk, medium-risk, or high-risk before acting:
    * **Low-risk:** Agent executes autonomously and logs the action. Human is notified but not blocked.
    * **Medium-risk:** Agent presents a structured recommendation and waits for human confirmation.
    * **High-risk:** Agent halts, presents full context and options, and requires explicit human sign-off before proceeding.
* Risk classification criteria must be defined per role by the governing BREQ of that role.

### Level 3 — Autonomous Delegate
* The agent executes all decisions within its delegated role scope independently.
* Every action is logged in real time to the Global History Ledger with an explicit flag marking it as an autonomous agent decision.
* All Level 3 actions are compiled into a structured review package and presented to the human operator at the next available session before the project may advance to the next phase gate.

## 4. Adaptive Capability Assessment Protocol

### 4.1 Observable Signal Tracking
The agent continuously monitors the following operator interaction signals across sessions:
* Frequency of clarification requests initiated by the operator.
* Consistency of operator decisions against established framework rules.
* Depth and quality of context provided by the operator in declarations and sign-offs.
* Rate of operator overrides on agent recommendations.

### 4.2 Level Adjustment Suggestion Rules
* The agent may issue a capability level adjustment suggestion only when a sustained pattern has been observed across a minimum of three sessions.
* The suggestion must be presented as a single structured proposal — never as repeated prompts.
* The operator may accept, reject, or defer the suggestion. All three responses are valid and logged. A deferred suggestion may be re-raised after three additional sessions.
* The agent never applies a level adjustment autonomously under any circumstance.

### 4.3 Profile Persistence
* The operator's interaction profile accumulates across sessions and is preserved in the Global History Ledger.
* The physical persistence mechanism for operator profiles is deliberately left undefined at the business layer and must be resolved via an ADR before implementation.
* Upon session initialization the agent loads the operator's existing profile before assuming any role.

## 5. Operator Identity & Pair Mode Protocol

### 5.1 Operator Handoff
* Any human operator may declare an identity change at any point in a session.
* The incoming operator must explicitly identify themselves before the framework resumes any activity.
* Upon identity declaration:
    * The outgoing operator's session state is preserved and logged.
    * If the incoming operator has a registered profile, it loads automatically.
    * If the incoming operator is new to the system, their profile initializes at Level 1 regardless of their declared experience level.
* The handoff event is logged in the Global History Ledger with both operator identities and timestamp.

### 5.2 Pair Mode
* Two human operators may declare a shared session by explicitly activating Pair Mode.
* Pair Mode activation requires both operators to identify themselves before the session resumes.
* In Pair Mode:
    * All sign-off events require dual signatures from both registered operators.
    * The Agent HITL adapts its capability level to the lower of the two operators' registered levels.
    * Either operator may independently trigger a TQUERY or escalation at any time.
    * Either operator may independently deactivate Pair Mode, reverting to single-operator session under their own identity.
* All Pair Mode activations, decisions, and deactivations are logged in the Global History Ledger with both operator identities.

## 6. Business Compliance & QA Metrics
* **Metric 1 (Delegation Declaration Coverage):** 100% of agent HITL assignments must have a complete delegation declaration logged before the agent assumes the role. Zero undeclared assignments permitted.
* **Metric 2 (Handoff Completeness):** 100% of revocation events must be accompanied by a structured handoff summary delivered to the incoming human operator.
* **Metric 3 (Autonomous Action Flagging):** 100% of Level 3 autonomous decisions must carry an explicit agent-decision flag in the Global History Ledger.
* **Metric 4 (Profile Load Verification):** 100% of session initializations must confirm operator profile load before any role assumption proceeds.
* **Metric 5 (Pair Mode Audit Coverage):** 100% of decisions made in Pair Mode must carry dual operator signatures in the Global History Ledger.
