# TEST TICKET: TEST_010 — Agent System Prompt Modules
**Parent DEV:** DEV_010 | **Parent TREQ:** TREQ_011

## 1. Test Goal
Verify all agent prompt modules conform to required structure
and stay within token budget.

## 2. Test Environment
Mock ProjectState and distillation objects. Tiktoken for counting.

## 3. Implementation Plan
* Unit: all four modules expose get_system_prompt(state, distillation)
* Unit: each module declares AGENT_ID and GOVERNING_BREQ
* Unit: output of each stays under 2,000 tokens with realistic state
* Unit: shared helpers produce consistent format across modules
