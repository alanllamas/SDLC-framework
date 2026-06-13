# DEV TICKET: DEV_031 — Commit Structure & Pull Request Creation
**version_tag:** v0.9.0 — Phase 2 Execution: Developer Workflow
**Parent TREQ:** TREQ_032
**Trace:** @trace TREQ_032

## 1. Development Process Boundaries
* Implement COMMIT_EDITMSG template write at ticket start
* Implement commit message prefix validation — prompt to
  prepend [ticket_id] if missing
* Add create_pull_request() to GitAdapter ABC
* Implement GitHubAdapter.create_pull_request() via GitHub API
  (requests, not SDK)
* Implement MockGitAdapter.create_pull_request() per TREQ_004
  mock pattern
* Implement complete_ticket() — status update, governance commit,
  optional PR creation, TICKET_COMPLETED Ledger entry

## 2. Acceptance Criteria
* Non-conforming commit message triggers prefix prompt before commit
* create_pull_request() implemented for both real and mock adapters
* complete_ticket() updates status to DONE and commits with
  correct message prefix
* PR created with ticket title and acceptance criteria
* TICKET_COMPLETED logged regardless of PR outcome

## 3. Pre-Defined Testing Assignment
See TEST_031

## 4. Documentation Assignment
User manual placeholder: "Completing a Ticket and Opening a PR"
