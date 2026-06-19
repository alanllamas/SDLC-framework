# DEV TICKET: DEV_045 — CostEstimator, CostTracker & Budget Enforcement
**version_tag:** v0.14.0 — Epistemic Charter & Cost Visibility
**Parent TREQ:** TREQ_047
**Trace:** @trace TREQ_047

## 1. Development Process Boundaries
* Implement CostEstimator per TREQ_047:
  - tiktoken cl100k_base for token counting
  - TASK_OUTPUT_AVERAGES dict with initial values
  - update_average() with EMA alpha=0.3
* Implement CostTracker per TREQ_047:
  - record() accumulates session + project costs
  - budget_status() returns OK/WARNING/CRITICAL/EXCEEDED
  - Persists project_cost_usd in state.yaml between sessions
* Implement enforce_budget() + budget options presentation
* Implement /cost Orchestrator command using render_cost_summary()
* Add PRICE_IN, PRICE_OUT, SESSION_BUDGET_USD, PROJECT_BUDGET_USD,
  COST_CONFIRM_THRESHOLD to .butler.env template
* Ledger entry on EXCEEDED override: event="BUDGET_OVERRIDE",
  details=cost context

## 2. Acceptance Criteria
* CostEstimator.estimate() tokens_in matches tiktoken count
* CostTracker accumulates correctly across multiple calls
* budget_status() thresholds: WARNING at 80%, CRITICAL at 95%,
  EXCEEDED at 100%
* Budget EXCEEDED always presents 3 options — never hard block
* EXCEEDED override logged to Ledger
* /cost responds in <100ms (local computation only)
* After 10 real calls of a task_type, estimates within 30%
  of real cost 80% of the time

## 3. Pre-Defined Testing Assignment
See TEST_045

## 4. Documentation Assignment
User manual placeholder: "Understanding and Controlling Costs"
