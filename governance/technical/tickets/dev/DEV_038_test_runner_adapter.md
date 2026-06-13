# DEV TICKET: DEV_038 — Test Runner Adapter
**version_tag:** v0.12.0 — Agent 9: Compliance Gatekeeper
**Parent TREQ:** TREQ_039
**Trace:** @trace TREQ_039

## 1. Development Process Boundaries
* Implement TestRunnerAdapter ABC per TREQ_039
* Implement PytestRunnerAdapter — subprocess pytest execution,
  300s timeout, exception translation to AdapterResult
* Implement parse_pytest_summary() — regex parsing of pytest
  summary line
* Implement MockTestRunnerAdapter per TREQ_004 mock pattern
* Register both in Adapter Registry (TREQ_003) via .butler.env

## 2. Acceptance Criteria
* PytestRunnerAdapter correctly parses passed/failed from real
  pytest output
* Timeout returns AdapterResult(success=False, ...), never hangs
* Subprocess exceptions never propagate raw
* MockTestRunnerAdapter returns configured values
* Adapter selectable via .butler.env

## 3. Pre-Defined Testing Assignment
See TEST_038

## 4. Documentation Assignment
User manual placeholder: "Configuring the Test Runner"
