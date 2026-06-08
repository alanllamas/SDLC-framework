---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_010
type: PREQ
title: "Innovation Proposal & Project Registry Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_001, BREQ_009, BREQ_016]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "PM Orchestrator", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Product Requirement: PREQ_010 — Innovation Proposal & Project Registry Flow

## 1. User Story / Description
As a Developer Solo, I need a clear way to propose innovations that travel up
the planning pipeline for evaluation, and to manage my active projects in a
single place — so good ideas are never lost and I always know what projects
I have running.

## 2. Behavioral & UI Flow Logic

### 2.1 Innovation Proposal Submission
Structured capture: idea description, origin layer, perceived impact.
System confirms capture and explains pipeline routing via BREQ_009.

### 2.2 Innovation Pipeline Status
Operator queries status: current layer, enrichments, rejections.
Business Catalyst may override any downstream rejection generating a BDR.

### 2.3 Project Registry — New Project Registration
Registry consultation per BREQ_016 before formalizing scope.
Overlap detection with three resolution options if found.
Registration upon HITL sign-off if clear.

### 2.4 Project Registry — Status Management
ACTIVE → PAUSED, ACTIVE → ARCHIVED (per BDR_017), PAUSED → ACTIVE with resume summary.
All changes logged in Global History Ledger.

### 2.5 Project Registry — Query
Full registry view with project ID, status, scope summary, last updated, active branch count.
Operator may drill in or switch active workspace.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1:** Innovation captured with pipeline routing explanation.
* **Scenario 2:** Pipeline status shows current layer, enrichments, rejections.
* **Scenario 3:** No overlap → scope formalization with registry consultation confirmation.
* **Scenario 4:** Overlap → specific intersection and three resolution options.
* **Scenario 5:** Status change → BDR_017 confirmation for ARCHIVED, resume summary for ACTIVE.
* **Scenario 6:** Registry query → all projects in single scannable response.
* **Scenario 7:** Business Catalyst override → BDR generated per BREQ_009 with audit trail.
