# TEST TICKET: TEST_031 — Commit Structure & PR Creation
**Parent DEV:** DEV_031 | **Parent TREQ:** TREQ_032

## 1. Test Goal
Verify commit message validation, PR creation across real and
mock adapters, and ticket completion flow.

## 2. Test Environment
Mock GitHub API responses, MockGitAdapter, temporary git repo.

## 3. Implementation Plan
* Unit: commit message without [ticket_id] prefix triggers prompt
* Unit: commit message with correct prefix proceeds without prompt
* Unit: GitHubAdapter.create_pull_request() returns AdapterResult
  with PR URL on success
* Unit: MockGitAdapter.create_pull_request() returns configurable
  success/failure
* Unit: complete_ticket() updates status to DONE and commits
  with [ticket_id] prefix
* Integration: full ticket completion cycle — status update,
  commit, PR creation, TICKET_COMPLETED logged
