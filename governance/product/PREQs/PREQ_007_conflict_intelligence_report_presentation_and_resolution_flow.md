---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_007
type: PREQ
title: "Conflict Intelligence Report Presentation & Resolution Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_003, BREQ_002, BREQ_006]
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

# Product Requirement: PREQ_007 — Conflict Intelligence Report Presentation & Resolution Flow

## 1. User Story / Description
As a Developer Solo, I need the system to present conflicts between planning layers
in a clear, structured format so that I can understand exactly what is in tension,
what each side proposes, and make an informed decision without reading multiple documents.

## 2. Behavioral & UI Flow Logic

### 2.1 Conflict Detection & Freeze
1. System freezes affected branch and announces:
   * Which two layers are in tension.
   * Which specific artifacts are involved.
   * One sentence describing the conflict nature.
2. Presents Conflict Intelligence Report in three-tier structure:
   * THE CONFLICT: Objective facts.
   * AGENT PROPOSALS: Non-binding solution paths, labeled as proposals.
   * YOUR DECISION: Explicit command slot requiring operator choice.
3. System enters waiting state until operator resolves.

### 2.2 Conflict Resolution
1. Operator selects resolution path or provides custom direction.
2. System confirms selection and consequence before executing.
3. Upon confirmation logs resolution, generates artifact, resumes branch.

### 2.3 Loop Identification
Clearly communicates active loop:
* Loop 1 — Business ↔ Product
* Loop 2 — Product ↔ Architecture
* Loop 3 — Architecture ↔ Implementation

### 2.4 Conflict History Query
Operator may request scannable summary of all conflicts at any time.

### 2.5 Escalation Path
If unresolvable at current loop, system announces escalation to next loop up.
Operator confirms before execution.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1 (Happy Path):** CIR presents facts, proposals, and decision slot in single response.
* **Scenario 2 (Resolution):** Consequence stated clearly before execution.
* **Scenario 3 (Loop ID):** Active loop clearly labeled with strategic context.
* **Scenario 4 (History):** All conflicts listed with loop, layers, status, resolution.
* **Scenario 5 (Escalation):** Operator confirms before execution, payload carries forward.
* **Scenario 6 (Error):** System blocks if no decision provided before branch unfreeze.
