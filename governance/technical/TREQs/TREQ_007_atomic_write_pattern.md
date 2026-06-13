---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_007
type: TREQ
title: "Atomic Write Pattern for State Files"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.1.0 — Bootstrap & Session Gateway"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_004
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_007 — Atomic Write Pattern

## 1. Intent & Scope
All state file writes use atomic temp-file-rename pattern
to prevent corruption on unexpected termination.

## 2. Technical Constraints
```python
import os
import tempfile

def atomic_write(path: str, content: str) -> None:
    dir_path = os.path.dirname(path)
    with tempfile.NamedTemporaryFile(
        mode='w', dir=dir_path, delete=False, suffix='.tmp'
    ) as tmp:
        tmp.write(content)
        tmp_path = tmp.name
    os.replace(tmp_path, path)
```
* Applies to: state.yaml, context.md, project_registry.md, profiles/[id].yaml.
* Never write directly to target file path.
* os.replace() atomic on Linux/WSL per POSIX guarantee.

## 3. Verification & QA Criteria
* No direct file writes to state files — all via atomic pattern.
* Simulated crash during write leaves file in last valid state.
* Unit test verifies atomic behavior via mocked os.replace.
