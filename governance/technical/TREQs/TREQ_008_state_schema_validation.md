---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_008
type: TREQ
title: "State Schema Validation by Librarian"
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

# Technical Requirement: TREQ_008 — State Schema Validation

## 1. Intent & Scope
Librarian must validate state.yaml against declared schema
before every write per BREQ_010.

## 2. Technical Constraints
* Schema validation via pydantic v2 models matching ADR_004 state.yaml structure.
* Validation runs before atomic write — invalid state never touches disk.
* Validation errors generate TQUERY via BREQ_012 pipeline.
* Schema versioned in /governance/templates/.

## 3. Verification & QA Criteria
* Invalid state.yaml rejected 100% of the time before write.
* Valid state.yaml passes validation in < 100ms.
* Schema version mismatch detected and surfaced to operator.
