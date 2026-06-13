# DEV TICKET: DEV_033 — BUG/DEBT Ticket Types & Closeout Surfacing
**version_tag:** v0.9.0 — Phase 2 Execution: Developer Workflow
**Parent TREQ:** TREQ_034
**Trace:** @trace TREQ_034

## 1. Development Process Boundaries
* Extend DEV_TICKET schema with type field (FEATURE/BUG/DEBT)
* Implement librarian.create_dev_ticket() for BUG (depends_on
  originating ticket) and DEBT (version_tag.milestone: BACKLOG)
* Extend generate_icd() (TREQ_027) with PROPOSED ticket scan
  for Technical layer closeouts — informational section only
* Implement HITL triage flow — promote PROPOSED to AUTHORIZED
  with milestone assignment

## 2. Acceptance Criteria
* Schema accepts type: BUG and type: DEBT
* BUG tickets carry correct depends_on
* DEBT tickets carry version_tag.milestone: BACKLOG
* ICD includes PROPOSED section when applicable, does not
  affect overall_status
* Promoted tickets appear in list_available_tickets() (TREQ_031)

## 3. Pre-Defined Testing Assignment
See TEST_033

## 4. Documentation Assignment
User manual placeholder: "Triaging Bugs and Technical Debt"
