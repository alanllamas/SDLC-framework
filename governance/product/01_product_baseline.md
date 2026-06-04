# Product Layer Index — 01_product_baseline.md
> Layer Index Document per BDR_016. Living document —
> never sealed, never AUTHORIZED. Maintained exclusively
> by the PM Orchestrator via the Librarian.

**Last Updated:** 2026-06-03T00:00:00Z
**Layer Owner:** PM Orchestrator (Agent 2)

---

## 1. Layer Mission
The Product Layer translates authorized business
requirements into atomic, user-facing product
specifications. It defines how the platform behaves
for the operator — never how it is built technically.
It does not own commercial strategy, database schemas,
or infrastructure decisions.

---

## 2. Active Artifacts Index

| ID | Title | Status | File |
|:---|:------|:-------|:-----|
| PDR_001 | Interaction Model & Operator Experience Mandate | AUTHORIZED | /governance/product/PDRs/PDR_001_interaction_model_and_operator_experience_mandate.md |
| PREQ_001 | Session Initialization & Operator Onboarding Flow | AUTHORIZED | /governance/product/PREQs/PREQ_001_session_initialization_and_operator_onboarding_flow.md |
| PREQ_002 | Agent Role Activation & Transition Flow | AUTHORIZED | /governance/product/PREQs/PREQ_002_agent_role_activation_and_transition_flow.md |
| PREQ_003 | TQUERY Presentation & Resolution Flow | AUTHORIZED | /governance/product/PREQs/PREQ_003_tquery_presentation_and_resolution_flow.md |
| PREQ_005 | Batch Closeout & Phase Transition Flow | AUTHORIZED | /governance/product/PREQs/PREQ_005_batch_closeout_and_phase_transition_flow.md |
| PREQ_006 | Document Generation & HITL Sign-off Flow | AUTHORIZED | /governance/product/PREQs/PREQ_006_document_generation_and_hitl_signoff_flow.md |

**Deprecated:**
| PREQ_004 | Document Generation & HITL Sign-off Flow | DEPRECATED | /governance/bprops/rollbacks/PREQ_004_document_generation_stale.md |

---

## 3. Accumulated Intelligence Ledger

* **2026-06-03** — Primary operator for v1.0 is Developer
  Solo (Config 2). All PREQs scoped to single-operator
  experience. Multi-user and team configs deferred to v1.1+.
* **2026-06-03** — Interaction model is conversational and
  platform-agnostic. Delivery mechanism (CLI vs UI) deferred
  to Architect via TQ_002.
* **2026-06-03** — Future Horizon items captured at each
  layer must stay in that layer's index — never cross-modify
  other layers' documents. Routing via BREQ_009 for upward
  escalation.
* **2026-06-03** — Draft retention is explicit-rejection-only.
  Micro-commits on every significant exchange as safety net.

---

## 4. Future Horizon Ledger
> Informal ideas only. Not backlog. Not committed.
> Activated only by explicit HITL decision.

* **Meeting Intelligence Assistant** — Framework extends
  governance to the full meeting lifecycle: automatic
  capture of decisions made in meetings, generation of
  structured minutes with traceability to affected
  governance artifacts, detection of commitments and
  actionable tracking, and status reports ready to
  present to stakeholders — all without additional
  administrative overhead for the team. Surfaces naturally
  as a complement to the passive listening and capture
  capabilities explored in the backlog.

* **Multi-organization operator support** — A developer
  operating across multiple organizations or client accounts
  needs the framework to maintain separate operator contexts
  per organization while sharing a single local installation.

* **Extended operator flow PREQs (v1.1)** — User-facing
  flows for each agent's internal work: Business Catalyst
  elicitation flow, PM requirement mapping flow, Architect
  evaluation flow, Tech Lead task breakdown flow.

---

## 5. Inherited TQUERYs

| ID | Description | Resolution Owner | Status |
|:---|:------------|:----------------|:-------|
| TQ_002 | Delivery mechanism — CLI local with LLM API, local repo and credential discovery | System Architect | PENDING |
