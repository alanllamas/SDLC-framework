---
extends_schema: "/governance/templates/master_metadata.yaml"
id: ADR_005
type: ADR
title: "Project Registry & Operator Profile Implementation"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_014
    parent_id: null
  horizontal:
    depends_on: [ADR_001, ADR_002, BDR_017]
  cross_plane:
    architecture_adr: [ADR_001]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: "Replaces DRAFT — TQ_007 resolved. Unified ~/.sdlc/ structure
        simultaneously resolves ADR_006 (Operator Profile Persistence)."

provenance_ledger:
  generation_layer: { agent_identity: "System Architect", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Architectural Decision Record: ADR_005 — Project Registry & Operator Profile Implementation

## 1. Context & Problem Statement
BDR_014 and BREQ_016 require a Project Registry accessible across all projects.
BDR_013 and BREQ_015 require operator profiles persisting between sessions.
Both are resolved simultaneously via a unified ~/.sdlc/ operator home directory.
This ADR also resolves ADR_006 (previously deferred).

## 2. Decision Outcome
* **Chosen Option:** Unified ~/.sdlc/ operator home directory.
* **Status:** AUTHORIZED
* **Resolves:** TQ_007, ADR_006

## 3. The ~/.sdlc/ Directory Structure

```
~/.sdlc/
├── project_registry.md      # Lightweight project index
├── profiles/
│   └── [operator_id].yaml   # Operator profile per BREQ_015
└── projects/
    └── [PRJ_ID]/
        └── context.md       # Extended project context
```

### 3.1 project_registry.md
Lightweight scannable index per BREQ_016.
Access via read_registry() and write_registry() adapters per ADR_001.

### 3.2 profiles/[operator_id].yaml
Persistent operator profile per BDR_013 and BREQ_015.
Stores capability level, active config, interaction history.
Exportable for multi-machine setup or team template sharing.

### 3.3 projects/[PRJ_ID]/context.md
Extended project context beyond the lightweight registry entry.

## 4. Security & Isolation Rules
* ~/.sdlc/ never tracked by any project Git repository.
* Never subject to BDR_017 — local operator configuration.
* Credential files remain in project roots per ADR_002 section 4.4.
* Git Butler initializes ~/.sdlc/ on first run per BREQ_017 Step 1.

## 5. Consequences & Impact
* TQ_007 Resolved.
* ADR_006 Resolved — no separate ADR needed.
* BREQ_016 and BREQ_015 unblocked.
* Tech Lead must implement: read_registry(), write_registry(),
  read_profile(), write_profile(), read_context(), write_context().

## 6. Future Horizon Notes
* SDLC Profile Portability — export/import ~/.sdlc/profiles/ for
  new machine setup or team template sharing.
* Multi-machine sync — requires dedicated ADR.
* Team registry — deferred to Config 1 planning.
