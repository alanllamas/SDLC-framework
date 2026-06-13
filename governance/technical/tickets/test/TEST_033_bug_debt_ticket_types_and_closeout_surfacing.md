# TEST TICKET: TEST_033 — BUG/DEBT Ticket Types & Closeout Surfacing
**Parent DEV:** DEV_033 | **Parent TREQ:** TREQ_034

## 1. Test Goal
Verify ticket type schema extension, ICD surfacing, and
HITL triage promotion flow.

## 2. Test Environment
Temporary DEV_TICKET set including PROPOSED BUG and DEBT tickets.

## 3. Implementation Plan
* Unit: DEV_TICKET front-matter accepts type: BUG and type: DEBT
* Unit: create_dev_ticket(type=BUG) sets depends_on to
  originating ticket
* Unit: create_dev_ticket(type=DEBT) sets
  version_tag.milestone=BACKLOG
* Unit: generate_icd() includes PROPOSED section listing BUG/DEBT
  tickets when present
* Unit: PROPOSED section presence does not change overall_status
* Integration: promoting a PROPOSED ticket to AUTHORIZED with a
  milestone makes it appear in list_available_tickets()
