# TEST TICKET: TEST_011 — Role Transition & Cooling-Off Gate
**Parent DEV:** DEV_011 | **Parent TREQ:** TREQ_012

## 1. Test Goal
Verify transition flow enforces cooling-off gate and never
mutates state without confirmation.

## 2. Test Environment
MockLLMAdapter, temporary state.yaml, mock Librarian.

## 3. Implementation Plan
* Unit: Transition Summary generated before state change
* Unit: rejection leaves state.yaml unchanged
* Unit: confirmation updates active_agent and active_layer atomically
* Unit: confirmed transition generates Ledger entry
* Unit: new agent's first response contains role announcement
* Integration: direct agent invocation attempt redirected to gate
* Integration: full transition cycle Business Catalyst → PM Orchestrator
