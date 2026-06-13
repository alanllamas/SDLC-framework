---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_019
type: TREQ
title: "CIR Dataclass, Three-Tier Construction & Loop Labeling"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.6.0 — Conflict Intelligence & History"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_010
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

# Technical Requirement: TREQ_019 — CIR Dataclass, Three-Tier Construction & Loop Labeling

## 1. Intent & Scope
Defines the CIR payload structure extending TREQ_013's
TQueryPayload pattern with three-tier content and loop labeling
per PREQ_007 sections 2.1 and 2.3.

## 2. Technical Constraints

```python
from pydantic import BaseModel
from typing import Literal

class AgentProposal(BaseModel):
    agent: str  # e.g., "System Architect"
    proposal: str  # non-binding solution path

class CIRPayload(BaseModel):
    id: str  # e.g., "CIR_001" — Librarian assigns sequentially
    loop: Literal["LOOP_1", "LOOP_2", "LOOP_3"]
    layers_involved: list[str]  # e.g., ["Product", "Architecture"]
    active_branch: str
    the_conflict: str  # objective facts — what each layer requires
    agent_proposals: list[AgentProposal]
    decision_slot_prompt: str  # what the operator is being asked
    related_artifacts: list[str]  # artifact IDs in tension
```

* Loop determined by `layers_involved`:
    * Business ↔ Product → LOOP_1
    * Product ↔ Architecture → LOOP_2
    * Architecture ↔ Implementation → LOOP_3
* `Librarian.write_cir(payload)` follows TREQ_013 pattern:
  validates, assigns sequential CIR_ID, atomic write to
  `governance/bprops/cirs/pending/`, updates
  `state.yaml.session.pending_cirs` (new array, parallel to
  `pending_tqueries`), freezes `active_branch`.

## 3. Verification & QA Criteria
* Loop correctly derived from layers_involved per mapping above.
* CIR_ID sequential, no collisions across pending/resolved.
* state.yaml.session.pending_cirs updated atomically with write.
* active_branch frozen — Orchestrator blocks further agent
  actions on that branch until CIR resolved.
