---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_025
type: TREQ
title: "Project Registry CRUD Operations"
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

# Technical Requirement: TREQ_025 — Project Registry CRUD Operations

## 1. Intent & Scope
Defines the structure of `~/.sdlc/project_registry.md` and the
CRUD operations the Librarian exposes per ADR_005.

## 2. Technical Constraints

### Registry File Structure
```markdown
# SDLC Project Registry
> Maintained by Librarian. Do not edit manually.

| project_id | name | repo_path | last_active | status |
|:-----------|:-----|:----------|:------------|:-------|
| PRJ_001 | SDLC Governance Framework | /home/alan/sdlc-framework | 2026-06-03T00:00:00Z | ACTIVE |
| PRJ_002 | Apogeo Leads | /home/alan/apogeo-leads | 2026-05-20T00:00:00Z | ACTIVE |
```

### CRUD Operations
```python
class ProjectEntry(BaseModel):
    project_id: str
    name: str
    repo_path: str
    last_active: str
    status: Literal["ACTIVE", "ARCHIVED"]

def list_projects(librarian: "Librarian") -> list[ProjectEntry]:
    """Parses project_registry.md table into typed entries."""

def register_project(
    name: str, repo_path: str, librarian: "Librarian"
) -> "AdapterResult":
    """
    Assigns next sequential PRJ_ID, appends row to registry table,
    creates ~/.sdlc/projects/[PRJ_ID]/ directory with initial
    state.yaml (per ADR_004 schema) and empty context.md.
    Atomic write per TREQ_007.
    """

def archive_project(project_id: str, librarian: "Librarian") -> "AdapterResult":
    """
    Sets status=ARCHIVED in registry row. Does NOT delete
    ~/.sdlc/projects/[PRJ_ID]/ — per BDR_020 non-destructive
    principle. Archived projects excluded from default
    list_projects() unless include_archived=True.
    """
```

## 3. Verification & QA Criteria
* list_projects() correctly parses table rows into ProjectEntry list.
* register_project() assigns next sequential ID without collision.
* register_project() creates valid initial state.yaml matching ADR_004 schema.
* archive_project() never deletes project directory — status flag only.
* Archived projects excluded from default listing.
