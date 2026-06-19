---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_046
type: TREQ
title: "OllamaAdapter & HybridLLMAdapter Implementation"
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

# Technical Requirement: TREQ_046 — OllamaAdapter & HybridLLMAdapter

## 1. Intent & Scope
Defines the implementation of the local model adapter
(OllamaAdapter) and the routing wrapper (HybridLLMAdapter)
per TDR_025 and BREQ_024.

## 2. Technical Constraints

```python
import httpx
from typing import Literal, Callable
from pydantic import BaseModel

# ── OllamaAdapter ──────────────────────────────────────────

class OllamaAdapter(LLMAdapter):
    """Local model via Ollama HTTP API.
    Default endpoint: http://localhost:11434
    Model configured in .butler.env: LOCAL_MODEL=qwen2.5-coder:7b
    """
    def __init__(self, base_url: str = "http://localhost:11434",
                 model: str = "qwen2.5-coder:7b"):
        self.base_url = base_url
        self.model = model

    def invoke(self, prompt: str, system: str = "") -> AdapterResult:
        try:
            resp = httpx.post(
                f"{self.base_url}/api/chat",
                json={
                    "model": self.model,
                    "messages": [
                        {"role": "system", "content": system},
                        {"role": "user", "content": prompt}
                    ],
                    "stream": False
                },
                timeout=120.0
            )
            resp.raise_for_status()
            data = resp.json()
            return AdapterResult(
                success=True,
                data=data["message"]["content"],
                tokens_in=data.get("prompt_eval_count", 0),
                tokens_out=data.get("eval_count", 0)
            )
        except httpx.ConnectError:
            return AdapterResult(
                success=False,
                error="LOCAL_UNAVAILABLE: Ollama not running at "
                      f"{self.base_url}. Start with: ollama serve"
            )
        except Exception as e:
            return AdapterResult(success=False, error=str(e))

    def is_available(self) -> bool:
        try:
            httpx.get(f"{self.base_url}/api/tags", timeout=2.0)
            return True
        except Exception:
            return False


# ── HybridLLMAdapter ───────────────────────────────────────

TaskTier = Literal["LOCAL", "CLOUD", "AMBIGUOUS"]

class HybridLLMAdapter:
    """Routes LLM calls between local (Ollama) and cloud (Claude)
    per BREQ_024 task tier classification."""

    def __init__(
        self,
        local: OllamaAdapter,
        cloud: LLMAdapter,           # existing ClaudeAdapter
        cost_estimator: "CostEstimator",
        config: "ButlerConfig"
    ):
        self.local = local
        self.cloud = cloud
        self.estimator = cost_estimator
        self.config = config

    def invoke(
        self,
        tier: TaskTier,
        prompt: str,
        system: str,
        task_type: str,
        state: "ProjectState",
        confirm_fn: Callable | None = None
    ) -> AdapterResult:

        # Resolve AMBIGUOUS
        if tier == "AMBIGUOUS":
            tier = self.config.ambiguous_tier_preference  # "LOCAL"|"CLOUD"

        # LOCAL path
        if tier == "LOCAL":
            if not self.local.is_available():
                # Visible fallback — never silent
                fallback_notice = (
                    "⚠  Local model unavailable — routing to CLOUD."
                )
                estimate = self.estimator.estimate(prompt, task_type)
                if confirm_fn:
                    confirmed = confirm_fn(estimate, state.cost_tracker,
                                          notice=fallback_notice)
                    if not confirmed:
                        return AdapterResult(
                            success=False,
                            error="Operation cancelled — local model "
                                  "unavailable and CLOUD not confirmed."
                        )
                tier = "CLOUD"  # fall through to CLOUD path
            else:
                return self.local.invoke(prompt, system)

        # CLOUD path
        estimate = self.estimator.estimate(prompt, task_type)

        if estimate.cost_usd >= self.config.cost_confirm_threshold:
            if confirm_fn:
                confirmed = confirm_fn(estimate, state.cost_tracker)
                if not confirmed:
                    return AdapterResult(
                        success=False,
                        error="CLOUD operation cancelled by operator."
                    )

        result = self.cloud.invoke(prompt, system)

        if result.success:
            # Update real-time tracker and refine estimate averages
            state.cost_tracker.record(
                tokens_in=result.tokens_in,
                tokens_out=result.tokens_out,
                task_type=task_type
            )
            self.estimator.update_average(
                task_type, result.tokens_out)

        return result
```

* `httpx` replaces `requests` for async-readiness and better
  timeout handling — single new dependency per TDR_025.
* `OllamaAdapter.is_available()` uses a 2-second probe timeout —
  fast enough for UX, won't hang the session if Ollama is absent.
* All Ollama errors translate to `AdapterResult(success=False)`
  per AREQ_002 — never raw exceptions.

## 3. Verification & QA Criteria
* OllamaAdapter returns LOCAL_UNAVAILABLE when Ollama is not
  running — never raises an unhandled exception.
* HybridLLMAdapter fallback from LOCAL to CLOUD is always
  visible to the operator — the notice is displayed before
  the confirm_fn call.
* CLOUD path records real tokens to CostTracker after every
  successful call.
* AMBIGUOUS tier resolves from .butler.env config — never
  hardcoded.
* is_available() completes in <2 seconds regardless of
  Ollama state.
