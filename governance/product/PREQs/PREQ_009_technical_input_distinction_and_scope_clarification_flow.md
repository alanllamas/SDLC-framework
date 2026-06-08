---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_009
type: PREQ
title: "Technical Input Distinction & Scope Clarification Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_001, BREQ_001]
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

# Product Requirement: PREQ_009 — Technical Input Distinction & Scope Clarification Flow

## 1. User Story / Description
As a Developer Solo, I need the system to intercept any technical stack mention
I make during planning and help me classify it correctly — hard constraint or
flexible preference — so I never accidentally lock downstream agents into a
technology choice I only intended as a suggestion.

## 2. Behavioral & UI Flow Logic

### 2.1 Technical Input Interception
System scans for necessity keywords. If found — routes as constraint.
If absent or ambiguous — freezes and presents clarification gate.

### 2.2 The Clarification Gate
Structured presentation:
* DETECTED: "You mentioned [technology]."
* QUESTION: "Is this a hard requirement or a preference?"
* Option A — Hard Constraint: locked downstream.
* Option B — Flexible Preference: logged in Vision Ledger.
Operator selects — no free-text at this gate.

### 2.3 Routing Outcomes
* Hard Constraint: added to active requirements with constraint flag.
* Flexible Preference: stripped from constraints, appended to Vision Ledger
  in 01_business_request.md with PREFERENCE tag.

### 2.4 Anti-Goal Scan
All technical inputs scanned against active Anti-Goals before routing.
Violations blocked with clear explanation and two options.

### 2.5 Scope Conflict Detection
New hard constraints immediately scanned against existing constraints.
Conflicts trigger PREQ_007 flow before proceeding.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1:** Necessity keyword present → direct constraint routing, no gate.
* **Scenario 2:** Ambiguous input → clarification gate presented in same response.
* **Scenario 3:** Preference selected → Vision Ledger confirmation before proceeding.
* **Scenario 4:** Anti-goal violation → blocked with explanation and two options.
* **Scenario 5:** Constraint conflict → CIR per PREQ_007 before acceptance.
* **Scenario 6 (Error):** Free-text at gate → system extracts intent and confirms before routing.
