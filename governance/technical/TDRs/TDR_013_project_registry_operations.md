---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_013
type: TDR
title: "Project Registry Operations & Active Project Switching"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.7.0 — Innovation Proposal & Project Registry"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_004, TDR_006]
  cross_plane:
    architecture_adr: ADR_005
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_013 — Project Registry Operations & Active Project Switching

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_005 — Project Registry & Operator Profile
  Implementation
* **Decision Scope:** How `~/.sdlc/project_registry.md` is read,
  updated, and how switching the active project isolates session
  state per PREQ_010 section 2.2.

## 2. The Implementation Decision
* **Selected Approach:** `project_registry.md` is a structured
  markdown table maintained by the Librarian — one row per project
  with `project_id`, `name`, `repo_path`, `last_active`, `status`
  (ACTIVE | ARCHIVED). CRUD operations
  (`list_projects()`, `register_project()`, `archive_project()`)
  read/write this table via atomic writes per TREQ_007.

  Switching the active project is a state isolation operation:
  the Orchestrator's in-memory session (current `state.yaml`,
  context distillation) is fully flushed to
  `~/.sdlc/projects/[OLD_PRJ_ID]/` before loading
  `~/.sdlc/projects/[NEW_PRJ_ID]/state.yaml` and its distillation.
  No cross-project state ever coexists in memory.

* **Rationale:** A markdown table keeps the registry human-readable
  and git-diffable, consistent with the framework's
  human-readable-audit-trail principle (TDR_004 rationale).
  Full flush-before-load on switch guarantees AREQ_001's
  "no partial state" guarantee extends across project boundaries
  — switching projects mid-session can never corrupt either
  project's state.

## 3. Evaluated Alternatives
### Option A — Markdown table registry + full flush-on-switch (Selected)
* **Pros:** Human-readable, git-diffable, strict state isolation,
  reuses atomic write pattern.
* **Cons:** Switching projects has a small flush latency —
  negligible for CLI interactive use.

### Option B — In-memory multi-project session cache
* **Pros:** Faster switching.
* **Cons:** Violates ADR_003 single-process-single-context model,
  risk of state bleed between projects, contradicts AREQ_001
  no-partial-state guarantee across project boundaries.

## 4. AREQ Boundary Compliance
* All registry reads/writes via RegistryAdapter per TREQ_003,
  atomic per TREQ_007.
* Flush-before-load uses same atomic write pattern as all other
  state.yaml operations.

## 5. Architect Validation
* **Validation required:** NO — implements ADR_005 structures
  already authorized.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_025 (Project Registry CRUD),
  TREQ_026 (active project switching & state isolation)
* **DEV_TICKETs impacted:** Project Registry implementation
