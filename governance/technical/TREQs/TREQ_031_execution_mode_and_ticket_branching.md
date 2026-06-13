---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_031
type: TREQ
title: "Execution Mode Session & Ticket-Driven Branching"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.9.0 — Phase 2 Execution: Developer Workflow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_016
  cross_plane:
    architecture_adr: ADR_002
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_031 — Execution Mode Session & Ticket-Driven Branching

## 1. Intent & Scope
Defines how the Orchestrator enters Execution Mode after Phase 1
closes, and how Git Butler creates ticket-driven branches.

## 2. Technical Constraints

### Execution Mode Entry
```python
def offer_execution_mode(state: "ProjectState") -> bool:
    return state.phase_gates.phase_1_closed and not state.phase_gates.phase_2_closed
```
* On entry, Orchestrator lists available DEV_TICKETs:
```python
def list_available_tickets(librarian: "Librarian") -> list[dict]:
    tickets = librarian.scan_dev_tickets()
    return [t for t in tickets
            if t["status"] == "AUTHORIZED"
            and t.get("execution_status", "PENDING") == "PENDING"]
```

### Ticket Selection & Branch Creation
```python
def start_ticket(
    ticket_id: str, git_butler: "GitButler", librarian: "Librarian"
) -> "AdapterResult":
    ticket = librarian.read_dev_ticket(ticket_id)
    slug = slugify(ticket["title"])
    branch_name = f"dev/{ticket_id}-{slug}"
    result = git_butler.branch(branch_name)
    if not result.success:
        return result

    # Update execution_status
    librarian.update_ticket_execution_status(ticket_id, "IN_PROGRESS")
    librarian.log_ledger_entry(
        event="TICKET_STARTED",
        artifact_id=ticket_id,
        details=f"branch: {branch_name}"
    )
    return AdapterResult(success=True, data=branch_name)
```

* `state.yaml` gains `session.execution_mode: bool` and
  `session.active_ticket: str | null`.
* While `active_ticket` is set, Orchestrator's context assembly
  (TDR_003) includes the ticket's full content in the active
  artifact slot instead of a governance document.

## 3. Verification & QA Criteria
* list_available_tickets() excludes tickets already IN_PROGRESS or DONE.
* start_ticket() creates branch named exactly `dev/[ID]-[slug]`.
* Branch creation failure returns error, execution_status unchanged.
* Successful start: execution_status updated, TICKET_STARTED
  Ledger entry generated.
* While a ticket is active, context assembly substitutes ticket
  content into active artifact slot.
