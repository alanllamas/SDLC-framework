---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_025
type: TDR
title: "HybridLLMAdapter — Local/Cloud Routing, Cost Estimation & Budget Tracking"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.14.0 — Epistemic Charter & Cost Visibility"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_001, TDR_002, TREQ_003, TREQ_004]
  cross_plane:
    architecture_adr: ADR_001
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-14T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# TDR_025 — HybridLLMAdapter

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_001 — Vendor-Agnostic Architecture
* **Governing BREQ:** BREQ_024 — Hybrid LLM Cost Model
* **Decision Scope:** Extend the existing LLMAdapter ABC
  (TREQ_003) to support simultaneous local (Ollama) and
  cloud (Claude API) instances, pre-operation cost estimation,
  real-time budget accumulation, and task-tier routing.

## 2. The Implementation Decision

### Selected Approach
Three new components, all within the existing adapter boundary:

**1. OllamaAdapter** — new LLMAdapter implementation.
  Connects to a local Ollama instance via HTTP (default
  http://localhost:11434). Returns AdapterResult with same
  interface as ClaudeAdapter. If Ollama is not running,
  returns AdapterResult(success=False, error="LOCAL_UNAVAILABLE")
  — Orchestrator falls back to CLOUD with a warning,
  never silently.

**2. CostEstimator** — stateless utility, not an adapter.
  Takes prompt string + task_type → CostEstimate dataclass.
  Token counting via tiktoken (cl100k_base, close enough for
  estimation across models). Output token estimate from
  TASK_OUTPUT_AVERAGES dict, updated after each real call.

**3. HybridLLMAdapter** — wraps OllamaAdapter + ClaudeAdapter.
  Routes per task_tier, runs cost estimation and confirmation
  for CLOUD operations, updates CostTracker in state.yaml
  after each real call.

### Key Design Decisions

**Ollama fallback is always visible.** If LOCAL is unavailable
and Orchestrator falls back to CLOUD, the operator sees:
"⚠ Local model unavailable — routing to CLOUD (~$X.XXX)."
No silent fallback per BDR_017 (Non-Destructive Operations —
no silent state changes).

**Cost confirmation is opt-in per threshold.** Default
COST_CONFIRM_THRESHOLD=0.05 USD. Set to 0.00 to always
confirm, 999.00 to never. Configured per project in
.butler.env, not hardcoded.

**Task averages self-calibrate.** After each CLOUD call,
the real token_out count is used to update a rolling average
for that task_type (exponential moving average, alpha=0.3).
Estimates improve over time per project.

**Budget limits produce options, not hard blocks.** When
budget is reached, the operator gets three choices — not
an error. Per BDR_005, the HITL retains final authority.
The override is logged to the Ledger.

## 3. Evaluated Alternatives

### Option A — HybridLLMAdapter wrapping both adapters (Selected)
* **Pros:** Single interface for Orchestrator, self-calibrating
  estimates, budget visibility built-in, consistent with
  existing AREQ_002 adapter pattern.
* **Cons:** Slightly more complex than the current single-adapter
  setup. Ollama dependency is new (but optional — framework
  works without it, just CLOUD-only).

### Option B — Two separate adapters, routing at Orchestrator level
* **Pros:** Simpler each adapter.
* **Cons:** Cost tracking and routing logic scattered across
  Orchestrator rather than encapsulated in adapter layer.
  Violates single-responsibility principle per BDR_011.

### Option C — External cost tracking service
* **Pros:** Persistent across machines.
* **Cons:** New infrastructure, violates ADR_002
  no-additional-infrastructure principle for v1.0.

## 4. AREQ Boundary Compliance
* OllamaAdapter: HTTP call to localhost — FilesystemAdapter
  not involved, within adapter boundary.
* CostEstimator: pure computation, no adapter calls.
* HybridLLMAdapter: composes existing adapters per AREQ_002
  composition pattern.
* tiktoken: new dependency, Python-only, no infrastructure.
  Architect validation: within Python 3.11+ runtime boundary
  per TDR_001. No Loop 3 triggered.

## 5. Architect Validation
* **Validation required:** YES — new external dependency
  (tiktoken) and new adapter type (OllamaAdapter, first
  local model adapter).
* **Architect sign-off:** Alan Llamas via Console Approval
* **Finding:** tiktoken is pip-installable, no infrastructure,
  no network call in production (uses local vocab file).
  OllamaAdapter uses HTTP to localhost — no external network
  dependency. Both within ADR_001 constraints.

## 6. Downstream Impact
* **TREQs generated:** TREQ_046 (OllamaAdapter + HybridLLMAdapter
  implementation), TREQ_047 (CostEstimator + CostTracker +
  budget enforcement)
* **DEV_TICKETs:** DEV_044, DEV_045
* **TEST_TICKETs:** TEST_044, TEST_045
