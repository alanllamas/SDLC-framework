---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_029
type: TREQ
title: "Five-Plane Verification Engine"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_015
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

# Technical Requirement: TREQ_029 — Five-Plane Verification Engine

## 1. Intent & Scope
Implements the five BDR_018 verification planes as deterministic
functions over the TDR_011 dependency graph and existing
directories.

## 2. Technical Constraints

```python
from pydantic import BaseModel
from typing import Literal

class Finding(BaseModel):
    artifact_id: str
    plane: Literal[1, 2, 3, 4, 5]
    severity: Literal["RED", "YELLOW", "GREEN"]
    what: str
    why: str
    resolution_criteria: str

def plane_1_ascending_traceability(graph, librarian) -> list[Finding]:
    """Every artifact's parent_id chain must terminate at a BDR.
    Walk parent_id from each artifact; chain break before BDR
    (null parent_id on non-BDR, or cycle) -> RED finding."""

def plane_2_descending_traceability(graph, librarian) -> list[Finding]:
    """Every artifact must have >=1 graph child OR a valid
    SkipDeclaration in its layer's baseline doc. Missing child
    with SkipDeclaration but no HITL deferral justification for
    DEFERRED-reason skips -> YELLOW. Missing both -> RED
    (should already be caught at ICD time, but re-checked here
    for cross-layer drift since sealing)."""

def plane_3_horizontal_coverage(graph, librarian) -> list[Finding]:
    """cross_plane.architecture_adr and .technical_schema
    references must point to AUTHORIZED artifacts. Dangling or
    non-AUTHORIZED reference -> RED."""

def plane_4_pending_resolution(librarian) -> list[Finding]:
    """governance/bprops/{tqueries,cirs,bprops}/pending/ must
    all be empty. Any file present -> RED, artifact_id = pending
    item's ID."""

def plane_5_anti_goal_compliance(librarian) -> list[Finding]:
    """Re-scan Active Technical Constraints (TREQ_018) and all
    AUTHORIZED artifact scope statements against Anti-Goals
    (01_business_request.md section 3) using matches_anti_goal()
    from TREQ_018. Match -> YELLOW if not yet escalated via CIR,
    RED if escalated CIR exists but unresolved."""

def run_five_plane_audit(librarian) -> list[Finding]:
    graph = build_dependency_graph(librarian)  # from TREQ_022
    findings = []
    findings += plane_1_ascending_traceability(graph, librarian)
    findings += plane_2_descending_traceability(graph, librarian)
    findings += plane_3_horizontal_coverage(graph, librarian)
    findings += plane_4_pending_resolution(librarian)
    findings += plane_5_anti_goal_compliance(librarian)
    return findings
```

* Each plane function is independently testable and returns
  an empty list when no issues found for that plane.
* `run_five_plane_audit()` rebuilds the dependency graph once
  and shares it across planes 1-3 — single graph build per
  audit run per TDR_015 rationale.

## 3. Verification & QA Criteria
* Plane 1: artifact with parent_id chain reaching a BDR produces
  no finding; chain ending in null before a BDR produces RED.
* Plane 2: artifact with graph child produces no finding;
  artifact with valid non-DEFERRED skip produces no finding;
  DEFERRED skip without HITL justification produces YELLOW;
  no child and no skip produces RED.
* Plane 3: cross_plane reference to AUTHORIZED artifact produces
  no finding; dangling/non-AUTHORIZED reference produces RED.
* Plane 4: empty pending/ directories produce no findings;
  any pending file produces RED with correct artifact_id.
* Plane 5: constraint/scope not matching any Anti-Goal produces
  no finding; match without CIR produces YELLOW; match with
  unresolved escalated CIR produces RED.
* run_five_plane_audit() builds graph exactly once per call.
