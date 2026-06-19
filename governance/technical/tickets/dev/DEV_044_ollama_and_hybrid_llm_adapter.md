# DEV TICKET: DEV_044 — OllamaAdapter & HybridLLMAdapter
**version_tag:** v0.14.0 — Epistemic Charter & Cost Visibility
**Parent TREQ:** TREQ_046
**Trace:** @trace TREQ_046

## 1. Development Process Boundaries
* Implement OllamaAdapter per TREQ_046:
  - httpx POST to /api/chat
  - is_available() with 2-second probe timeout
  - All errors → AdapterResult(success=False), never raw exceptions
  - LOCAL_UNAVAILABLE error message includes "ollama serve" hint
* Implement HybridLLMAdapter per TREQ_046:
  - AMBIGUOUS resolution from .butler.env config
  - LOCAL→CLOUD fallback always visible (notice in confirm_fn)
  - CLOUD path triggers CostEstimator + confirm_fn before calling
  - Records real tokens to CostTracker after successful CLOUD call
* Register OllamaAdapter in Adapter Registry per TREQ_003
  (.butler.env: LOCAL_MODEL=qwen2.5-coder:7b)
* Add httpx to project dependencies (replaces requests in
  OllamaAdapter; existing GitHubAdapter can stay on requests
  for now — migration optional)

## 2. Acceptance Criteria
* OllamaAdapter returns LOCAL_UNAVAILABLE when Ollama not running,
  never raises unhandled exception
* HybridLLMAdapter LOCAL→CLOUD fallback always shows notice before
  confirm_fn — zero silent fallbacks
* CLOUD path records real token counts to CostTracker on success
* AMBIGUOUS resolves from .butler.env, not hardcoded
* is_available() completes in <2 seconds in all states

## 3. Pre-Defined Testing Assignment
See TEST_044

## 4. Documentation Assignment
User manual placeholder: "Setting Up the Local Model (Ollama)"
