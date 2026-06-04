---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_001
type: PREQ
title: "Session Initialization & Operator Onboarding Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [BREQ_014, BREQ_015]
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

# Product Requirement: PREQ_001 — Session Initialization & Operator Onboarding Flow

## 1. User Story / Description
As a Developer Solo, I need the system to recognize whether
I am a new or returning operator and guide me accordingly
so that I can start working immediately without having to
remember where I left off or how the system works.

## 2. Behavioral & UI Flow Logic

### 2.1 First-Time Operator Flow
1. System detects no existing operator profile.
2. System identifies itself and explains its purpose
   in plain language — maximum 3 sentences.
3. System initiates operator registration:
   * Asks for operator name/identifier.
   * Asks which HITL operational config they want
     to use (Config 1, 2, 3, or 4) with a plain
     language explanation of each option.
   * Asks for capability level preference if Config
     3 or 4 is selected (Level 1, 2, or 3).
4. System confirms registration and explains the
   next step — starting a new project or loading
   an existing one.
5. System presents two options:
   * Start a new project (triggers Project Registry
     consultation per BREQ_016).
   * Load an existing project from a repository path.
     The system must attempt to recognize existing
     repositories and credentials available in the
     local environment before asking the operator
     to provide them manually. (Note for Architect:
     mechanism for local repo and credential
     discovery to be defined via ADR — see TQ_002.)

### 2.2 Returning Operator Flow
1. System detects existing operator profile.
2. System greets operator by name and loads their
   profile including capability level and active
   configuration.
3. System presents a session state summary:
   * Active project name and current phase.
   * Last action completed.
   * Pending TQUERYs awaiting resolution.
   * Suggested next action.
4. Operator confirms to continue or selects an
   alternative starting point.

### 2.3 Interrupted Session Recovery
1. System detects a previously interrupted session
   with unsaved in-progress work.
2. System presents the interrupted state clearly:
   * Which document was in progress.
   * What decision was pending.
3. Operator chooses to resume or discard the
   interrupted state.
4. If discarded, the interrupted state is archived
   in `/governance/bprops/rollbacks/` before clearing.

### 2.4 Config Query
1. At any point the operator may request their
   current configuration.
2. System presents operator profile, active config,
   capability level, and active project in a single
   structured response without interrupting the
   active flow.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1 (Happy Path — New Operator):**
  Given the system has no existing operator profile,
  When the operator starts a session,
  Then the system completes registration and presents
  a clear next step in under 5 exchanges.

* **Scenario 2 (Happy Path — Returning Operator):**
  Given the operator has an existing profile and an
  active project,
  When the operator starts a session,
  Then the system presents a complete session state
  summary and suggested next action in under 2
  exchanges.

* **Scenario 3 (Interrupted Session):**
  Given a previously interrupted session exists,
  When the operator starts a session,
  Then the system surfaces the interrupted state
  and offers resume or discard before proceeding.

* **Scenario 4 (Error — No Project Registry):**
  Given the operator selects "load existing project"
  but no Project Registry is found,
  Then the system explains the gap clearly and offers
  to initialize a new Project Registry before
  proceeding.

* **Scenario 5 (Config Query):**
  Given the operator is at any point in any session,
  When the operator requests their current configuration,
  Then the system presents operator profile, active
  config, capability level, and active project in
  a single structured response without interrupting
  the active flow.
