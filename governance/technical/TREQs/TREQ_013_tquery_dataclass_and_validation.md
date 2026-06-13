---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_013
type: TREQ
title: "TQUERY Dataclass & Librarian Validation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.3.0 — TQUERY Flow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_007
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

# Technical Requirement: TREQ_013 — TQUERY Dataclass & Validation

## 1. Intent & Scope
Defines the typed structure agents use to construct TQUERY
payloads in-memory, and the Pydantic validation Librarian
applies before writing per BREQ_012.

## 2. Technical Constraints
```python
from pydantic import BaseModel
from typing import Literal

class ResolutionOwner(BaseModel):
    agent_layer: str
    hitl_role: str | None = None
    hitl_signatory: str | None = None
    fallback: Literal["HITL"] = "HITL"

class TQueryPayload(BaseModel):
    id: str  # e.g., "TQ_016" — Librarian assigns next sequential ID
    trigger_type: Literal["KNOWLEDGE_GAP", "SCOPE_DRIFT"]
    originating_agent: str
    active_branch: str
    file_in_focus: str
    problem_statement: str
    resolution_owner: ResolutionOwner
    actionable_options: dict[str, str | None]  # option_alpha, option_beta, option_custom
```

* `TQueryPayload` constructed by originating agent — no file I/O.
* `Librarian.write_tquery(payload)`:
    1. Validates against schema — rejects if any required field missing.
    2. Assigns next sequential TQ_ID by scanning
       `/governance/bprops/tqueries/` (both pending/ and resolved/).
    3. Serializes to YAML matching `TQUERY_metadata.yaml` template.
    4. Atomic write to `pending/TQ_NNN_[slug].yaml` per TREQ_007.
    5. Updates `state.yaml.session.pending_tqueries` — appends ID.
* On validation failure: Librarian returns error to Orchestrator,
  which surfaces it to operator — TQUERY never written.

## 3. Verification & QA Criteria
* Invalid payload (missing required field) rejected before write.
* TQ_ID assigned correctly avoiding collisions with existing files.
* Written YAML matches TQUERY_metadata.yaml schema exactly.
* state.yaml.session.pending_tqueries updated atomically with file write.
