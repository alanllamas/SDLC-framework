---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_047
type: TREQ
title: "CostEstimator, CostTracker & Budget Enforcement"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.14.0 — Epistemic Charter & Cost Visibility"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_025
  cross_plane:
    architecture_adr: ADR_001
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-14T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_047 — CostEstimator, CostTracker & Budget Enforcement

## 1. Intent & Scope
Defines cost estimation, real-time accumulation, and budget
enforcement per BREQ_024 sections 3-5.

## 2. Technical Constraints

```python
import tiktoken
from pydantic import BaseModel
from typing import Literal

# ── Pricing (configurable in .butler.env) ─────────────────
# Defaults match Claude Sonnet 4.6 as of 2026-06-14
DEFAULT_PRICE_IN  = 3.00 / 1_000_000   # $ per input token
DEFAULT_PRICE_OUT = 15.00 / 1_000_000  # $ per output token

# Initial output token averages per task type
TASK_OUTPUT_AVERAGES: dict[str, int] = {
    "BDR_BREQ_GENERATION":     800,
    "PDR_PREQ_GENERATION":     600,
    "ADR_AREQ_GENERATION":    1200,
    "TDR_GENERATION":         1500,
    "TREQ_GENERATION":         800,
    "CHRONICLE_GENERATION":    800,
    "COMPLIANCE_REPORT":       600,
    "ELICITATION_TURN":        400,
    "ONTOLOGY_DIGEST_PHASE2":  500,
    "RELEASE_NOTES":           700,
    "TQUERY_GENERATION":       400,
    "GENERAL":                 600,  # fallback
}

# ── CostEstimate ──────────────────────────────────────────

class CostEstimate(BaseModel):
    tokens_in: int
    tokens_out_estimated: int
    cost_usd: float
    task_type: str

# ── CostEstimator ─────────────────────────────────────────

class CostEstimator:
    """Stateless utility. Estimates CLOUD call cost before execution."""

    def __init__(self, price_in: float = DEFAULT_PRICE_IN,
                 price_out: float = DEFAULT_PRICE_OUT):
        self.enc = tiktoken.get_encoding("cl100k_base")
        self.price_in = price_in
        self.price_out = price_out
        # Mutable averages — updated per project via update_average()
        self.averages = dict(TASK_OUTPUT_AVERAGES)

    def estimate(self, prompt: str, task_type: str) -> CostEstimate:
        tokens_in = len(self.enc.encode(prompt))
        tokens_out = self.averages.get(task_type,
                     self.averages["GENERAL"])
        cost = (tokens_in * self.price_in +
                tokens_out * self.price_out)
        return CostEstimate(
            tokens_in=tokens_in,
            tokens_out_estimated=tokens_out,
            cost_usd=round(cost, 6),
            task_type=task_type
        )

    def update_average(self, task_type: str, real_tokens_out: int,
                       alpha: float = 0.3) -> None:
        """Exponential moving average — calibrates over time."""
        current = self.averages.get(task_type,
                  self.averages["GENERAL"])
        self.averages[task_type] = int(
            alpha * real_tokens_out + (1 - alpha) * current
        )

# ── CostTracker ───────────────────────────────────────────

class CostTracker(BaseModel):
    session_tokens_in: int = 0
    session_tokens_out: int = 0
    session_cost_usd: float = 0.0
    project_cost_usd: float = 0.0   # persisted in state.yaml
    budget_session_usd: float | None = None
    budget_project_usd: float | None = None

    def record(self, tokens_in: int, tokens_out: int,
               task_type: str,
               price_in: float = DEFAULT_PRICE_IN,
               price_out: float = DEFAULT_PRICE_OUT) -> None:
        cost = (tokens_in * price_in + tokens_out * price_out)
        self.session_tokens_in += tokens_in
        self.session_tokens_out += tokens_out
        self.session_cost_usd = round(self.session_cost_usd + cost, 6)
        self.project_cost_usd = round(self.project_cost_usd + cost, 6)

    def budget_status(self) -> Literal["OK", "WARNING", "CRITICAL", "EXCEEDED"]:
        """Returns worst-case status across session and project budgets."""
        pct = max(
            self._pct(self.session_cost_usd, self.budget_session_usd),
            self._pct(self.project_cost_usd, self.budget_project_usd)
        )
        if pct is None:   return "OK"
        if pct >= 1.0:    return "EXCEEDED"
        if pct >= 0.95:   return "CRITICAL"
        if pct >= 0.80:   return "WARNING"
        return "OK"

    def _pct(self, spent: float, budget: float | None) -> float | None:
        if budget is None or budget == 0:
            return None
        return spent / budget

# ── /cost command output ──────────────────────────────────

def render_cost_summary(tracker: CostTracker,
                        estimator: CostEstimator,
                        pending_tickets: int = 0) -> str:
    session_pct = ""
    if tracker.budget_session_usd:
        pct = tracker.session_cost_usd / tracker.budget_session_usd * 100
        session_pct = f" ({pct:.0f}% de ${tracker.budget_session_usd:.2f})"

    project_pct = ""
    if tracker.budget_project_usd:
        pct = tracker.project_cost_usd / tracker.budget_project_usd * 100
        project_pct = f" ({pct:.0f}% de ${tracker.budget_project_usd:.2f})"

    milestone_est = ""
    if pending_tickets > 0:
        avg_per_ticket = (
            estimator.averages["TDR_GENERATION"] * DEFAULT_PRICE_OUT +
            estimator.averages["CHRONICLE_GENERATION"] * DEFAULT_PRICE_OUT
        )
        est = pending_tickets * avg_per_ticket
        milestone_est = f"\n   Estimado para completar milestone: ~${est:.3f}"

    return (
        f"💰 Costo actual\n"
        f"   Sesión:   ${tracker.session_cost_usd:.4f}{session_pct}\n"
        f"   Tokens:   {tracker.session_tokens_in:,} in  |  "
        f"{tracker.session_tokens_out:,} out\n"
        f"   Proyecto: ${tracker.project_cost_usd:.4f}{project_pct}"
        f"{milestone_est}"
    )
```

### Budget enforcement behavior
```python
def enforce_budget(
    tracker: CostTracker,
    estimate: CostEstimate,
    orchestrator: "Orchestrator"
) -> Literal["PROCEED", "WARN_AND_PROCEED", "CONFIRM_REQUIRED",
             "OFFER_OPTIONS"]:
    status = tracker.budget_status()
    if status == "EXCEEDED":
        orchestrator.present_budget_options(tracker, estimate)
        return "OFFER_OPTIONS"
    if status == "CRITICAL":
        return "CONFIRM_REQUIRED"
    if status == "WARNING":
        orchestrator.show_warning(
            f"Llevas el {tracker._pct(tracker.session_cost_usd, tracker.budget_session_usd)*100:.0f}%"
            f" de tu budget de sesión."
        )
        return "WARN_AND_PROCEED"
    return "PROCEED"
```

### Budget options presented on EXCEEDED
```
⚠  Budget de sesión alcanzado ($X.XX de $Y.YY).

Opciones:
A) Continuar sin límite (override — se registra en Ledger)
B) Cambiar operaciones pendientes a LOCAL donde sea posible
C) Pausar sesión — reanudar con budget fresco en nueva sesión
```

## 3. Verification & QA Criteria
* CostEstimator.estimate() produces tokens_in matching real
  tiktoken count for the same prompt.
* CostTracker.record() accumulates session and project costs
  correctly across multiple calls.
* budget_status() returns EXCEEDED when spent >= budget.
* WARNING fires at 80%, CRITICAL at 95% — not before.
* /cost output renders in <100ms (pure local computation).
* Budget EXCEEDED always presents three options — never a
  hard block per BDR_005 HITL sovereignty.
* EXCEEDED override is logged to Ledger with cost context.
* Self-calibrating averages: after 10 real calls of a task_type,
  estimates are within 30% of real cost 80% of the time.
