---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_014
type: TDR
title: "Batch Closeout & Layer Transition Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_004, TDR_006, TDR_008]
  cross_plane:
    architecture_adr: ADR_004
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_014 — Batch Closeout & Layer Transition

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine, section 3.3
  (layer transitions gated by ICD status).
* **Decision Scope:** How an active agent's layer work is closed
  out, how the Ingestion Compliance Declaration (ICD) is generated
  and presented for HITL sign-off, and how `state.yaml.phase_gates`
  and `ingestion_compliance` are updated to unlock the next layer,
  per PREQ_005 and BDR_020.

## 2. The Implementation Decision
* **Selected Approach:** Closeout is agent-initiated when the
  active agent determines its layer's planned artifacts are
  AUTHORIZED. The Orchestrator runs the **ICD Generation Engine**
  (TREQ_027) — a deterministic scan producing a completeness
  check, coverage check (including Skip Declarations per BDR_020),
  and conflict pre-scan. Results classify as `PASSED`,
  `PASSED_WITH_FLAGS` (yellow items present), or `FAILED` (red —
  missing required artifacts or unresolved blocking skips).

  - `FAILED`: closeout blocked, Orchestrator surfaces exactly what's
    missing — no state change.
  - `PASSED` / `PASSED_WITH_FLAGS`: ICD rendered for HITL review
    (same rendering pattern as document sign-off, TDR_008).
    On HITL confirmation, `Librarian.seal_layer()` (TREQ_028)
    executes as one atomic transaction: appends ICD section to
    the current layer's `01_*_baseline.md`, sets
    `phase_gates.[layer]_sealed = true`,
    `ingestion_compliance.[layer]_to_[next_layer] = PASSED |
    PASSED_WITH_FLAGS`, Ledger entry, and triggers the role
    transition (TDR_006/TREQ_012) to the next layer's agent.

* **Rationale:** Reuses the ICD format already established
  manually throughout v0.1.0-v0.7.0 planning — this milestone
  makes that manual process deterministic and code-enforced.
  Reusing TDR_008's render-for-review pattern and TDR_006's
  transition mechanics avoids inventing new presentation or
  state-change primitives.

## 3. Evaluated Alternatives
### Option A — Deterministic ICD engine + atomic seal + existing transition flow (Selected)
* **Pros:** Codifies an already-proven manual process, reuses
  TDR_006/TDR_008 primitives, single atomic seal operation.
* **Cons:** None significant.

### Option B — HITL-only closeout (no automated ICD generation)
* **Pros:** Simpler implementation.
* **Cons:** Reintroduces the manual-checklist risk BDR_020 was
  designed to eliminate — defeats the purpose of this milestone.

## 4. AREQ Boundary Compliance
* All scans are pure computation over filesystem-read artifacts
  via FilesystemAdapter — no adapter calls beyond reads.
* Seal operation atomic per TREQ_007.

## 5. Architect Validation
* **Validation required:** NO — implements ADR_004 section 3.3
  transitions already authorized.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_027 (ICD Generation Engine),
  TREQ_028 (Layer Seal & Phase Transition State Updates)
* **DEV_TICKETs impacted:** Closeout and transition implementation
