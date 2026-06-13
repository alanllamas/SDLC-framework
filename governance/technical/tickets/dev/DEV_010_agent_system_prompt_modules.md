# DEV TICKET: DEV_010 — Agent System Prompt Modules
**version_tag:** v0.2.0 — Agent Role Activation & Transitions
**Parent TREQ:** TREQ_011
**Trace:** @trace TREQ_011

## 1. Development Process Boundaries
* Implement /src/agents/_shared.py with format_decisions() and
  format_questions() helpers
* Implement business_catalyst.py, pm_orchestrator.py,
  system_architect.py, tech_lead.py per TREQ_011
* Each module declares AGENT_ID and GOVERNING_BREQ
* Each get_system_prompt() injects state and distillation

## 2. Acceptance Criteria
* All four modules implement identical get_system_prompt() signature
* Output of each stays within 2,000 token budget per TDR_003
* No adapter calls within any agent module
* Shared helpers produce consistent formatting across all agents

## 3. Pre-Defined Testing Assignment
See TEST_010

## 4. Documentation Assignment
User manual placeholder: "Understanding Agent Roles"
