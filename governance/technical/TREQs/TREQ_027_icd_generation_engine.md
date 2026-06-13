---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_027
type: TREQ
title: "ICD Generation Engine"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_014
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

# Technical Requirement: TREQ_027 — ICD Generation Engine

## 1. Intent & Scope
Defines the deterministic scan that produces an Ingestion
Compliance Declaration candidate for a layer closeout per
BDR_020 and PREQ_005.

## 2. Technical Constraints

```python
from typing import Literal
from pydantic import BaseModel

class SkipDeclaration(BaseModel):
    artifact_id: str
    reason: Literal["GOVERNANCE_POLICY", "DEFERRED",
                     "COVERED_BY_SIBLING", "TQUERY_PENDING"]
    reference: str | None = None  # sibling ID or TQ_ID

class ICDResult(BaseModel):
    upstream_layer: str
    receiving_layer: str
    artifacts_ingested: int
    completeness_check: Literal["PASSED", "FAILED"]
    coverage_check: Literal["PASSED", "PASSED_WITH_SKIPS", "FAILED"]
    skip_declarations: list[SkipDeclaration]
    inherited_skips_resolved: list[dict]
    conflict_prescan: Literal["PASSED", "FAILED"]
    conflicts: list[str]
    overall_status: Literal["PASSED", "PASSED_WITH_FLAGS", "FAILED"]

def generate_icd(
    layer: str,
    graph: dict,  # from build_dependency_graph() per TREQ_022
    librarian: "Librarian"
) -> ICDResult:
    artifacts = librarian.scan_layer_artifacts(layer)

    # Completeness: every artifact referenced by relationships
    # of artifacts in `artifacts` must exist and be AUTHORIZED
    completeness = check_referenced_artifacts_exist_and_authorized(
        artifacts, librarian)

    # Coverage: every artifact in `artifacts` without a graph
    # child (per TREQ_022 graph) must have a SkipDeclaration
    skips = []
    for artifact in artifacts:
        if artifact["id"] not in graph or not graph[artifact["id"]]:
            decl = librarian.read_skip_declaration(artifact["id"])
            if decl:
                skips.append(decl)
            else:
                completeness = "FAILED"  # uncovered, undeclared

    # Inherited skips: re-check skips from prior ICDs for resolution
    inherited = resolve_inherited_skips(layer, graph, librarian)

    # Conflict pre-scan: duplicate IDs, contradicting statuses
    conflicts = scan_for_conflicts(artifacts)

    overall = "FAILED" if completeness == "FAILED" or conflicts else (
        "PASSED_WITH_FLAGS" if skips or any(
            s["status"] == "CHALLENGED" for s in inherited
        ) else "PASSED"
    )

    return ICDResult(
        upstream_layer=layer,
        receiving_layer=next_layer(layer),
        artifacts_ingested=len(artifacts),
        completeness_check="PASSED" if completeness != "FAILED" else "FAILED",
        coverage_check="PASSED_WITH_SKIPS" if skips else (
            "FAILED" if completeness == "FAILED" else "PASSED"),
        skip_declarations=skips,
        inherited_skips_resolved=inherited,
        conflict_prescan="FAILED" if conflicts else "PASSED",
        conflicts=conflicts,
        overall_status=overall
    )
```

* `next_layer()` follows BDR_001 layer order:
  Business → Product → Architecture → Technical.
* `FAILED` overall_status blocks closeout — Orchestrator surfaces
  `conflicts` and uncovered/undeclared artifacts as an actionable
  list, no state change.

## 3. Verification & QA Criteria
* Artifact missing a referenced AUTHORIZED dependency → completeness FAILED.
* Artifact with no graph child and no SkipDeclaration → coverage FAILED,
  overall FAILED.
* Artifact with no graph child but valid SkipDeclaration → coverage
  PASSED_WITH_SKIPS.
* Duplicate artifact IDs detected → conflict_prescan FAILED.
* overall_status correctly derived per combination rules above.
