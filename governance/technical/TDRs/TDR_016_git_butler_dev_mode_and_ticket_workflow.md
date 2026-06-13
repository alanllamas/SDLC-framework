---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_016
type: TDR
title: "Git Butler Developer Mode & Ticket-Driven Branch Workflow"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.9.0 — Phase 2 Execution: Developer Workflow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_001, TDR_004, TDR_008, TDR_014]
  cross_plane:
    architecture_adr: ADR_002
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_016 — Git Butler Developer Mode & Ticket-Driven Branch Workflow

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_002 — Runtime Environment & Repository
  Bootstrap Protocol
* **Decision Scope:** How Git Butler operates once
  `phase_gates.phase_1_closed == true` (TREQ_030) — branch
  creation per DEV_TICKET, commit structure, and how the
  Orchestrator transitions from planning-mode to execution-mode
  sessions, per GAP_006 (now resolved by this milestone).

## 2. The Implementation Decision
* **Selected Approach:** Once Phase 1 closes, the Orchestrator
  offers a new session mode — **Execution Mode**. In Execution
  Mode:
  - Operator selects a DEV_TICKET (from
    `/governance/technical/tickets/dev/`, filtered to
    `status: AUTHORIZED` and not yet `IN_PROGRESS`/`DONE`).
  - Git Butler creates a branch named `dev/[DEV_TICKET_ID]-[slug]`
    from the current default branch.
  - Each commit during this ticket's work is structured:
    `[DEV_TICKET_ID] <description>` — enforced by a commit-msg
    template Git Butler writes to `.git/COMMIT_EDITMSG` defaults.
  - On ticket completion, operator confirms — Git Butler updates
    the DEV_TICKET's front-matter `status: DONE`, commits this
    update, and offers to open a PR (via GitAdapter
    `create_remote_repo`/branch operations already implemented
    in v0.1.0 — PR creation added as new GitAdapter method).
  - TEST_TICKET completion follows the same pattern, referencing
    the same branch.

* **Rationale:** Reuses the GitHubAdapter (DEV_004) and
  FilesystemAdapter (DEV_005) built in v0.1.0 — only a new
  `create_pull_request()` adapter method is needed. Ticket-driven
  branching keeps the Git history traceable directly to governance
  artifacts (DEV_TICKET IDs in branch names and commit messages)
  without inventing a parallel tracking system.

## 3. Evaluated Alternatives
### Option A — Ticket-driven branches + structured commits + PR creation (Selected)
* **Pros:** Direct traceability from Git history to governance
  artifacts, reuses existing adapters plus one new method,
  no new tracking system.
* **Cons:** None significant for v1.0 single-operator scope.

### Option B — Free-form developer workflow, Git Butler passive
* **Pros:** Maximum developer flexibility.
* **Cons:** Loses traceability between code changes and
  governance tickets — undermines BREQ_011's purpose.

## 4. AREQ Boundary Compliance
* New `create_pull_request()` added to `GitAdapter` ABC
  (extends TREQ_003) with corresponding mock per TREQ_004.
* DEV_TICKET status updates via Librarian, atomic per TREQ_007.

## 5. Architect Validation
* **Validation required:** NO — extends ADR_002 Git provider
  boundary with one additional adapter method, no new provider
  dependency.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_031 (Execution Mode session &
  ticket-driven branching), TREQ_032 (commit structure & PR
  creation adapter method)
* **DEV_TICKETs impacted:** Git Butler execution mode implementation
* **Resolves:** GAP_006 (Phase 2 execution support planning)
