# TEST TICKET: TEST_004 — GitHub Git Adapter
**Parent DEV:** DEV_004 | **Parent TREQ:** TREQ_003

## 1. Test Goal
Verify all Git operations work and credential discovery
follows ADR_002 order.

## 2. Test Environment
Mock GitPython and GitHub API for unit tests.
Integration test uses live GitHub — requires valid token.

## 3. Implementation Plan
* Unit: credential discovery checks env vars before .butler.env
* Unit: GitHub API exceptions translated to AdapterResult
* Integration: branch(), commit(), fetch_code_delta() work against real repo
