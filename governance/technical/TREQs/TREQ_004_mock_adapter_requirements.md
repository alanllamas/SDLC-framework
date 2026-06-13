---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_004
type: TREQ
title: "Mock Adapter Implementation Requirements"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_002
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

# Technical Requirement: TREQ_004 — Mock Adapter Requirements

## 1. Intent & Scope
Every production adapter has corresponding mock supporting
configurable responses and failures per AREQ_002 section 2.2.

## 2. Technical Constraints
* Mocks live exclusively in /src/adapters/mocks/.
* Configurable responses via constructor:
```python
class MockLLMAdapter(LLMAdapter):
    def __init__(self, responses: list[str], fail_after: int = -1):
        self.responses = responses
        self.call_count = 0
        self.fail_after = fail_after

    def invoke_llm(self, prompt: str, context: str) -> AdapterResult:
        if self.fail_after > 0 and self.call_count >= self.fail_after:
            return AdapterResult(success=False, error="Simulated API failure")
        response = self.responses[self.call_count % len(self.responses)]
        self.call_count += 1
        return AdapterResult(success=True, data=response)
```
* Mocks support: success responses, timeout, auth failure, rate limit.

## 3. Verification & QA Criteria
* 100% of production adapters have corresponding mocks.
* All failure scenarios testable via mock configuration.
* Full system operable with mock adapters — no live credentials.
