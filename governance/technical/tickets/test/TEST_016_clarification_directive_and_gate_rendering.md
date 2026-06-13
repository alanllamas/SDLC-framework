# TEST TICKET: TEST_016 — Clarification Directive & Gate Rendering
**Parent DEV:** DEV_016 | **Parent TREQ:** TREQ_017

## 1. Test Goal
Verify directive parsing, gate rendering, and reply validation
work correctly across all agent contexts.

## 2. Test Environment
MockLLMAdapter returning responses with/without directive blocks.

## 3. Implementation Plan
* Unit: response with <<CLARIFICATION_GATE>> block — directive
  parsed correctly, block stripped from visible output
* Unit: response without directive — passes through unchanged
* Unit: rendered gate contains exactly options A and B
* Unit: reply "a", "A", " A " all route to option A
* Unit: reply "xyz" triggers re-presentation with reminder
* Integration: each of the four agent prompts (TREQ_011) can
  emit a valid directive when given a technology-mention scenario
