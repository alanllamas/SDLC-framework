# TEST TICKET: TEST_044 — OllamaAdapter & HybridLLMAdapter
**Parent DEV:** DEV_044 | **Parent TREQ:** TREQ_046

## 1. Test Goal
Verify OllamaAdapter error handling, HybridLLMAdapter routing,
and zero-silent-fallback guarantee.

## 2. Test Environment
Mock Ollama server (httpx mock), MockClaudeAdapter,
temporary .butler.env with various AMBIGUOUS_TIER configs.

## 3. Implementation Plan
* Unit: OllamaAdapter — successful call returns AdapterResult
  with correct data
* Unit: OllamaAdapter — ConnectError returns LOCAL_UNAVAILABLE,
  never raises
* Unit: OllamaAdapter.is_available() returns False when Ollama
  not running, completes in <2s
* Unit: HybridLLMAdapter — tier=LOCAL routes to OllamaAdapter
* Unit: HybridLLMAdapter — tier=CLOUD routes to ClaudeAdapter
* Unit: HybridLLMAdapter — tier=AMBIGUOUS resolves from config
* Unit: LOCAL unavailable → fallback to CLOUD — notice present
  in confirm_fn args, never silent
* Unit: CLOUD path — real tokens recorded to CostTracker on success
* Unit: CLOUD path — CostTracker not updated on failure
* Integration: .butler.env LOCAL_MODEL config selects correct
  Ollama model in OllamaAdapter
