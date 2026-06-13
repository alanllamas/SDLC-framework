# DEV TICKET: DEV_030 — Execution Mode Session & Ticket-Driven Branching
**version_tag:** v0.9.0 — Phase 2 Execution: Developer Workflow
**Parent TREQ:** TREQ_031
**Trace:** @trace TREQ_031

## 1. Development Process Boundaries
* Implement offer_execution_mode() gate per TREQ_031
* Implement list_available_tickets() — scans DEV_TICKETs,
  filters AUTHORIZED + PENDING execution_status
* Implement start_ticket() — branch creation via Git Butler,
  execution_status update, TICKET_STARTED Ledger entry
* Add state.yaml.session.execution_mode and
  session.active_ticket fields
* Wire active ticket content into TDR_003 context assembly
  active artifact slot

## 2. Acceptance Criteria
* Execution Mode offered only when phase_1_closed and not
  phase_2_closed
* list_available_tickets() excludes IN_PROGRESS/DONE tickets
* start_ticket() creates branch dev/[ID]-[slug] exactly
* Branch failure leaves execution_status unchanged
* Successful start updates execution_status and logs
  TICKET_STARTED
* Active ticket content appears in context assembly active
  artifact slot

## 3. Pre-Defined Testing Assignment
See TEST_030

## 4. Documentation Assignment
User manual placeholder: "Starting Development Work on a Ticket"
