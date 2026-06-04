---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_002
type: PREQ
title: "Agent Role Activation & Transition Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_001, BREQ_014, BREQ_015]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "PM Orchestrator", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Product Requirement: PREQ_002 — Agent Role Activation & Transition Flow

## 1. User Story / Description
As a Developer Solo, I need the system to clearly signal
which agent role is active at any moment and guide me
through role transitions so that I always know who I am
talking to, what decisions belong to that role, and when
it is time to move to the next phase.

## 2. Behavioral & UI Flow Logic

### 2.1 Agent Role Activation
1. When a session begins or a phase transition occurs,
   the system explicitly announces the active agent role:
   * Role name and number (e.g., "Agent 1 — Business Catalyst")
   * A one-sentence description of what this role does.
   * The current phase objective.
2. The system presents the operator with the first
   actionable prompt for that role — never an open-ended
   "what do you want to do?"

### 2.2 Role Transition — Config 2 (Single Operator)
1. When the active agent role completes its phase objectives,
   the system announces the transition:
   * Summary of what was completed in the current role.
   * What the next role will do.
   * Cooling-off gate per BREQ_014 — operator must
     explicitly confirm the transition before proceeding.
2. Upon confirmation the system activates the next agent
   role and announces it as per 2.1.

### 2.3 Role Delegation — Config 3 (Agent HITL)
1. At any point the operator may declare delegation of
   a role to an agent HITL.
2. System confirms delegation parameters:
   * Which role is being delegated.
   * Capability level (1, 2, or 3).
   * Expiration condition if applicable.
3. System activates the delegated agent HITL and
   announces the change clearly.
4. Delegated agent HITL identifies itself differently
   from the primary assistant to avoid confusion:
   * Primary assistant: "Agent X — [Role Name]"
   * Delegated HITL: "Agent HITL — [Role Name] (Delegated)"

### 2.4 Role Interruption & Recovery
1. The operator may interrupt any active role at any
   time with a natural language command.
2. Upon interruption the system:
   * Preserves the current role state in memory.
   * Acknowledges the interruption clearly.
   * Presents options: resume current role, switch
     to a different role, or review session state.
3. If the operator switches roles mid-flow, the system
   flags the incomplete state and reminds the operator
   upon returning to that role.

### 2.5 Role Query
1. At any point the operator may ask which role is
   active and what its current objective is.
2. System responds with active role, current objective,
   and last completed action in a single structured
   response without interrupting the active flow.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1 (Happy Path — Role Activation):**
  Given a phase transition has been triggered,
  When the next agent role activates,
  Then the system announces the role, its purpose,
  and the first actionable prompt in a single response.

* **Scenario 2 (Happy Path — Config 2 Transition):**
  Given the Business Catalyst has completed its phase,
  When the system triggers a role transition,
  Then the cooling-off gate presents a completion
  summary and requires explicit operator confirmation
  before the PM Orchestrator activates.

* **Scenario 3 (Delegation — Config 3):**
  Given the operator declares role delegation,
  When the delegated agent HITL activates,
  Then the system clearly distinguishes the delegated
  agent from the primary assistant in all subsequent
  responses.

* **Scenario 4 (Interruption Recovery):**
  Given the operator interrupts an active role flow,
  When the operator returns to that role,
  Then the system surfaces the incomplete state and
  last pending decision before resuming.

* **Scenario 5 (Role Query):**
  Given the operator is at any point in a session,
  When the operator asks which role is active,
  Then the system responds with role, objective, and
  last action in a single structured response.

* **Scenario 6 (Error — Invalid Transition):**
  Given the operator attempts to skip a required phase,
  When the system detects the invalid transition,
  Then the system explains why the transition is blocked
  and presents the compliant path forward.
