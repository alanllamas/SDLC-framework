---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_003
type: PREQ
title: "TQUERY Presentation & Resolution Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_002, BREQ_012]
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

# Product Requirement: PREQ_003 — TQUERY Presentation & Resolution Flow

## 1. User Story / Description
As a Developer Solo, I need the system to present
blockers and decision points in a clear, scannable
format so that I can resolve them quickly without
losing context of what I was doing or why the system
stopped.

## 2. Behavioral & UI Flow Logic

### 2.1 TQUERY Trigger & Freeze
1. When an agent hits a block, the system immediately
   announces the freeze in plain language:
   * Which document or flow is paused.
   * Why it stopped — one sentence maximum.
2. The system presents the TQUERY in a structured
   format:
   * **BLOCKED:** [What is paused]
   * **WHY:** [Reason in plain business terms]
   * **YOUR OPTIONS:**
     * A) [Option Alpha — action and impact]
     * B) [Option Beta — action and impact]
     * C) [Custom — provide your own direction]
3. System enters explicit waiting state — no further
   actions until operator resolves the TQUERY.

### 2.2 TQUERY Resolution
1. Operator selects an option or provides custom input.
2. System confirms the selection and its immediate
   consequence before executing:
   * "You selected A. This will [consequence]. Confirm?"
3. Upon confirmation system logs the resolution with
   operator identity and timestamp and resumes the
   frozen flow from the exact point it stopped.

### 2.3 Multiple Pending TQUERYs
1. If more than one TQUERY is pending, the system
   presents them in priority order — blocking TQUERYs
   first, advisory TQUERYs second.
2. Operator resolves one at a time — system never
   presents more than one TQUERY simultaneously.
3. After each resolution the system confirms remaining
   pending count before presenting the next.

### 2.4 TQUERY Escalation
1. If a TQUERY cannot be resolved at the current
   agent layer, the system announces escalation:
   * Which layer is receiving the escalation.
   * What information is being passed up.
2. Operator confirms escalation before it executes.
3. System preserves the frozen state until the
   escalation is resolved and returns a determination.

### 2.5 TQUERY History Query
1. At any point the operator may request a list of
   all resolved and pending TQUERYs for the active
   session or project.
2. System presents a scannable summary — ID, type,
   status, and resolution owner — without expanding
   full detail unless requested.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1 (Happy Path — Single TQUERY):**
  Given an agent hits a knowledge gap,
  When the TQUERY is presented,
  Then the operator can read the block, reason, and
  options and make a decision in under 60 seconds.

* **Scenario 2 (Happy Path — Resolution Confirmation):**
  Given the operator selects an option,
  When the system confirms the selection,
  Then the consequence is stated clearly before
  execution and the operator must explicitly confirm.

* **Scenario 3 (Multiple TQUERYs):**
  Given more than one TQUERY is pending,
  When the operator enters resolution mode,
  Then the system presents exactly one TQUERY at a
  time in priority order with a pending count visible.

* **Scenario 4 (Escalation):**
  Given a TQUERY cannot be resolved at the current layer,
  When escalation is triggered,
  Then the system announces the target layer and
  requires operator confirmation before escalating.

* **Scenario 5 (TQUERY History):**
  Given the operator requests TQUERY history,
  When the system responds,
  Then all TQUERYs are listed with ID, type, status,
  and resolution owner in a single scannable response.

* **Scenario 6 (Error — Custom Input Insufficient):**
  Given the operator selects custom input but provides
  insufficient direction,
  When the system evaluates the input,
  Then the system asks one clarifying question rather
  than proceeding with an assumption.
