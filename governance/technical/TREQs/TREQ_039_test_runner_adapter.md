---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_039
type: TREQ
title: "Test Runner Adapter Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_022
  cross_plane:
    architecture_adr: ADR_001
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_039 — Test Runner Adapter Implementation

## 1. Intent & Scope
Defines the TestRunnerAdapter ABC, its pytest-based production
implementation, and mock per TDR_022.

## 2. Technical Constraints

```python
from abc import ABC, abstractmethod
import subprocess

class TestRunnerAdapter(ABC):
    @abstractmethod
    def run_tests(self, test_path: str) -> "AdapterResult":
        """data = {"passed": int, "failed": int, "output": str}"""
        ...

class PytestRunnerAdapter(TestRunnerAdapter):
    def run_tests(self, test_path: str) -> "AdapterResult":
        try:
            result = subprocess.run(
                ["pytest", test_path, "--tb=short", "-q"],
                capture_output=True, text=True, timeout=300
            )
            passed, failed = parse_pytest_summary(result.stdout)
            return AdapterResult(
                success=True,
                data={"passed": passed, "failed": failed,
                      "output": result.stdout}
            )
        except subprocess.TimeoutExpired:
            return AdapterResult(success=False,
                error="Test execution timed out after 300s")
        except Exception as e:
            return AdapterResult(success=False, error=str(e))

class MockTestRunnerAdapter(TestRunnerAdapter):
    def __init__(self, passed: int = 0, failed: int = 0, output: str = ""):
        self.passed = passed
        self.failed = failed
        self.output = output

    def run_tests(self, test_path: str) -> "AdapterResult":
        return AdapterResult(
            success=True,
            data={"passed": self.passed, "failed": self.failed,
                  "output": self.output}
        )
```

* `parse_pytest_summary()` parses pytest's standard summary line
  (e.g., `"3 passed, 1 failed in 0.42s"`) via regex.
* Added to Adapter Registry (TREQ_003) — `.butler.env` selects
  `pytest` or `mock` per existing pattern.
* `TEST_PATH` resolved per ticket: `tests/[DEV_TICKET_ID]/` or
  TEST_TICKET-declared path — convention established this
  milestone, documented in TREQ_040.

## 3. Verification & QA Criteria
* PytestRunnerAdapter correctly parses passed/failed counts from
  real pytest output.
* Timeout (300s) returns AdapterResult(success=False, ...) —
  never hangs Orchestrator.
* Subprocess exceptions translated to AdapterResult — never
  raw propagation per AREQ_002.
* MockTestRunnerAdapter returns configured values for test scenarios.
* Adapter selectable via .butler.env per TREQ_003 registry pattern.
