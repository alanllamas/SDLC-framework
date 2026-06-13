---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_002
type: TREQ
title: "Project Directory Structure"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_001
  cross_plane:
    architecture_adr: ADR_002
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_002 — Project Directory Structure

## 1. Intent & Scope
Mandatory source code directory structure per ADR_002 and AREQ_002.

## 2. Technical Constraints
```
/src/
├── main.py                  # CLI entry point
├── orchestrator/             # Per ADR_003
│   ├── __init__.py
│   └── orchestrator.py
├── adapters/                 # Per AREQ_002
│   ├── interfaces/
│   ├── implementations/
│   └── mocks/
├── librarian/                # Per BREQ_010
│   └── librarian.py
├── git_butler/               # Per BREQ_011
│   └── git_butler.py
├── state/                     # Per ADR_004
│   └── state_manager.py
└── agents/                    # Agent system prompts
    ├── business_catalyst.py
    ├── pm_orchestrator.py
    ├── system_architect.py
    └── tech_lead.py
```

## 3. Verification & QA Criteria
* Directory structure validated by Librarian on startup.
* Any deviation triggers governance alert.
