# TEST TICKET: TEST_045 — CostEstimator, CostTracker & Budget Enforcement
**Parent DEV:** DEV_045 | **Parent TREQ:** TREQ_047

## 1. Test Goal
Verify cost estimation accuracy, budget threshold behavior,
and /cost command output.

## 2. Test Environment
Sample prompts of known token lengths, mock CostTracker with
configurable budget values, mock Orchestrator for option presentation.

## 3. Implementation Plan
* Unit: CostEstimator.estimate() — tokens_in matches tiktoken count
  for identical prompt string
* Unit: CostEstimator.estimate() — task_type not in averages falls
  back to GENERAL
* Unit: CostEstimator.update_average() — EMA with alpha=0.3
  converges toward real values over 10 calls
* Unit: CostTracker.record() — session and project totals
  accumulate correctly
* Unit: CostTracker.budget_status() — OK below 80%, WARNING
  at 80%, CRITICAL at 95%, EXCEEDED at 100%
* Unit: enforce_budget() — EXCEEDED calls present_budget_options
  with all 3 choices
* Unit: EXCEEDED override → Ledger entry with event="BUDGET_OVERRIDE"
* Unit: render_cost_summary() — output includes session cost,
  tokens, project cost, optional milestone estimate
* Unit: /cost computation completes in <100ms (measured with
  time.perf_counter())
* Integration: full session with 5 CLOUD calls — project_cost_usd
  persists correctly in state.yaml between session restart
