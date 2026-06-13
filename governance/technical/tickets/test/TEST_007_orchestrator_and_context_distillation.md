# TEST TICKET: TEST_007 — Orchestrator & Context Distillation
**Parent DEV:** DEV_007 | **Parent TREQ:** TREQ_005, TREQ_006

## 1. Test Goal
Verify token budget enforcement and context distillation
update after every exchange.

## 2. Test Environment
MockLLMAdapter for all LLM calls. Tiktoken for token counting.

## 3. Implementation Plan
* Unit: context assembly never exceeds 9,500 tokens
* Unit: truncation removes from least recent exchanges first
* Unit: distillation updated after authorized artifact
* Unit: API failure detected within 10 seconds (mock timeout)
* Unit: state preserved before surfacing failure options
* Integration: full session with mock adapter produces valid distillation
