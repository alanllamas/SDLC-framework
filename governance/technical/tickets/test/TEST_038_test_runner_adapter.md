# TEST TICKET: TEST_038 — Test Runner Adapter
**Parent DEV:** DEV_038 | **Parent TREQ:** TREQ_039

## 1. Test Goal
Verify pytest execution, parsing, timeout handling, and mock
adapter behavior.

## 2. Test Environment
Temporary test directory with sample passing/failing pytest
files. Mocked subprocess for timeout scenario.

## 3. Implementation Plan
* Unit: run_tests() on passing suite returns
  {passed: N, failed: 0}
* Unit: run_tests() on suite with failures returns correct
  failed count
* Unit: parse_pytest_summary() correctly parses standard
  summary line formats
* Unit: simulated timeout returns AdapterResult(success=False,
  error contains "timed out")
* Unit: simulated subprocess exception returns AdapterResult
  with error, no raw exception
* Unit: MockTestRunnerAdapter returns configured passed/failed/output
* Integration: adapter selection via .butler.env switches
  between pytest and mock
