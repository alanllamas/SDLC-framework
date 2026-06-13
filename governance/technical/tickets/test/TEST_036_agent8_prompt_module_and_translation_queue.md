# TEST TICKET: TEST_036 — Agent 8 Prompt Module & Translation Queue
**Parent DEV:** DEV_036 | **Parent TREQ:** TREQ_037

## 1. Test Goal
Verify Agent 8 module conformance and event-driven queue
population.

## 2. Test Environment
MockLLMAdapter, temporary state.yaml, sample CHRONICLE
documents with varying consumed_by_agent_8 values.

## 3. Implementation Plan
* Unit: product_translator.py exposes get_system_prompt(state, distillation)
  matching TREQ_011 signature
* Unit: DOCUMENT_AUTHORIZED for type=CHRONICLE,
  consumed_by_agent_8=false → added to translation_queue
* Unit: DOCUMENT_AUTHORIZED for type=CHRONICLE,
  consumed_by_agent_8=true → not added
* Unit: DOCUMENT_AUTHORIZED for non-CHRONICLE type → not added
* Integration: operator confirmation triggers TREQ_012
  transition, role announcement present
