# TEST TICKET: TEST_030 — Execution Mode & Ticket-Driven Branching
**Parent DEV:** DEV_030 | **Parent TREQ:** TREQ_031

## 1. Test Goal
Verify Execution Mode gating, ticket listing, and branch
creation behave correctly.

## 2. Test Environment
Mock Git Butler, temporary state.yaml and DEV_TICKET set
with mixed statuses.

## 3. Implementation Plan
* Unit: offer_execution_mode() true only when
  phase_1_closed=true and phase_2_closed=false
* Unit: list_available_tickets() excludes IN_PROGRESS and DONE
* Unit: start_ticket() creates branch dev/[ID]-[slug]
* Unit: branch creation failure leaves execution_status unchanged
* Unit: successful start updates execution_status=IN_PROGRESS,
  logs TICKET_STARTED
* Integration: active ticket content appears in context
  assembly active artifact slot per TDR_003
