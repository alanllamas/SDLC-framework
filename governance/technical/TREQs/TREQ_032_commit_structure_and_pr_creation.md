---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_032
type: TREQ
title: "Commit Structure & Pull Request Creation"
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

// TODO: checar la sugerencia u obligacion del log, que no hace autotrigger, 
// TODO: revisar en donde se encuentra la carpeta /log

# Technical Requirement: TREQ_032 — Commit Structure & Pull Request Creation

## 1. Intent & Scope
Defines the commit message structure enforced during ticket
work, and adds PR creation to the GitAdapter ABC.

## 2. Technical Constraints

### Commit Structure
* Git Butler writes a `.git/COMMIT_EDITMSG` template at ticket
  start: `[{ticket_id}] `
* Commits during active ticket work are validated by Git Butler
  pre-commit — message must start with `[{active_ticket}]`.
  Non-conforming commit message → Orchestrator prompts to
  prepend the prefix before allowing commit via `commit()`.

### GitAdapter Extension
```python
class GitAdapter(ABC):
    # ... existing methods from TREQ_003 ...
    @abstractmethod
    def create_pull_request(
        self, branch: str, title: str, body: str, base: str = "main"
    ) -> AdapterResult: ...
```
* `GitHubAdapter.create_pull_request()` implemented via GitHub
  API per ADR_002 — uses `requests`, not SDK, consistent with
  DEV_004's `create_remote_repo()` pattern.
* `MockGitAdapter.create_pull_request()` returns configurable
  success/failure per TREQ_004 mock pattern.

### Ticket Completion Flow
```python
def complete_ticket(
    ticket_id: str, git_butler: "GitButler", librarian: "Librarian"
) -> "AdapterResult":
    librarian.update_ticket_status(ticket_id, status="DONE")
    librarian.commit_governance_update(
        message=f"[{ticket_id}] Mark ticket DONE")

    offer_pr = True  # Orchestrator asks operator
    if offer_pr:
        ticket = librarian.read_dev_ticket(ticket_id)
        pr_result = git_butler.create_pull_request(
            branch=f"dev/{ticket_id}-*",
            title=f"[{ticket_id}] {ticket['title']}",
            body=ticket["acceptance_criteria"]
        )
        return pr_result

    librarian.log_ledger_entry(event="TICKET_COMPLETED", artifact_id=ticket_id)
    return AdapterResult(success=True, data=ticket_id)
```

## 3. Verification & QA Criteria
* Commit message not starting with `[{active_ticket}]` triggers
  prefix prompt before commit proceeds.
* create_pull_request() implemented for both GitHubAdapter and
  MockGitAdapter.
* complete_ticket() updates status to DONE, commits governance
  change with correct message prefix.
* PR creation uses ticket title and acceptance criteria as
  title/body.
* TICKET_COMPLETED Ledger entry generated regardless of PR creation outcome.
