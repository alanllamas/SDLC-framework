---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_026
type: TREQ
title: "Active Project Switching & State Isolation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.7.0 — Innovation Proposal & Project Registry"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_013
  cross_plane:
    architecture_adr: ADR_005
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_026 — Active Project Switching & State Isolation

## 1. Intent & Scope
Defines the flush-before-load sequence for switching the active
project, guaranteeing no cross-project state contamination per
AREQ_001 and PREQ_010 section 2.2.

## 2. Technical Constraints

```python
def switch_active_project(
    new_project_id: str,
    orchestrator: "Orchestrator",
    librarian: "Librarian"
) -> "AdapterResult":
    current = orchestrator.current_project_id
    if current:
        # 1. Flush current session state — atomic write
        librarian.save_state(
            f"~/.sdlc/projects/{current}/state.yaml",
            orchestrator.current_state
        )
        librarian.save_context(
            f"~/.sdlc/projects/{current}/context.md",
            orchestrator.context_distillation
        )
        # 2. Update registry last_active for old project
        librarian.update_registry_last_active(current)

    # 3. Validate new project exists and is ACTIVE
    entry = librarian.get_project_entry(new_project_id)
    if not entry or entry.status != "ACTIVE":
        return AdapterResult(success=False,
            error=f"Project {new_project_id} not found or archived")

    # 4. Load new project's state and distillation
    new_state = librarian.load_state(
        f"~/.sdlc/projects/{new_project_id}/state.yaml")
    new_context = librarian.load_context(
        f"~/.sdlc/projects/{new_project_id}/context.md")

    # 5. Replace in-memory session — old state fully discarded
    #    from memory (already flushed in step 1)
    orchestrator.current_project_id = new_project_id
    orchestrator.current_state = new_state
    orchestrator.context_distillation = new_context

    # 6. Update registry last_active for new project
    librarian.update_registry_last_active(new_project_id)

    return AdapterResult(success=True, data=new_project_id)
```

* Steps 1-2 (flush) execute fully before steps 3-6 (load) begin
  — no partial overlap.
* If step 1 fails (write error), switch aborts — operator remains
  on current project with state intact.
* If steps 3-4 fail (new project invalid/unreadable), switch
  aborts — current project's state already safely flushed,
  operator can retry or stay.

## 3. Verification & QA Criteria
* Switching flushes old project's state.yaml and context.md
  before loading new project's.
* In-memory state after switch contains ONLY new project's data
  — no fields from previous project remain.
* Switch to archived or non-existent project_id rejected,
  current project state already safely flushed.
* Flush failure aborts switch — operator stays on current project.
* Both projects' last_active timestamps updated correctly.
