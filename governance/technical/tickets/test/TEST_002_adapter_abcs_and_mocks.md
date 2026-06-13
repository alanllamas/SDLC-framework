# TEST TICKET: TEST_002 — Adapter ABCs & Mocks
**Parent DEV:** DEV_002 | **Parent TREQ:** TREQ_003, TREQ_004

## 1. Test Goal
Verify all ABCs enforce interface contract and mocks
support configurable failure scenarios.

## 2. Test Environment
Unit tests — no live credentials needed.

## 3. Implementation Plan
* Unit: instantiating ABC directly raises TypeError
* Unit: MockLLMAdapter returns configured responses in order
* Unit: MockLLMAdapter returns failure after configured count
* Unit: AdapterResult fields typed correctly
* Integration: Adapter Registry loads correct implementation from env
