---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_001
type: TREQ
title: "Python Version & Dependency Pinning"
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

# Technical Requirement: TREQ_001 — Python Version & Dependency Pinning

## 1. Intent & Scope
Python 3.11+ minimum runtime version. All dependencies pinned
to exact versions in pyproject.toml for reproducible environments.

## 2. Technical Constraints
* Python >= 3.11 declared in pyproject.toml requires-python.
* All direct dependencies pinned with exact versions.
* uv or pip-tools for dependency lock file generation.
* .python-version file at project root.

## 3. Verification & QA Criteria
* CI verifies Python version on every push.
* Lock file committed to repository.
* Fresh install from lock file produces identical environment.
