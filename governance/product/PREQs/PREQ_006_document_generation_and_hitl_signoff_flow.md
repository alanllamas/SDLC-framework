---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_006
type: PREQ
title: "Document Generation & HITL Sign-off Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_003, BREQ_010, BREQ_011]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: "PREQ_004"
      link_reason: "Correct section 2.2 to mandate full
        document presentation with visual differentiation
        of modified content after every change request,
        replacing the previous partial-section-only rule
        that conflicted with Scenario 7."

provenance_ledger:
  generation_layer: { agent_identity: "PM Orchestrator", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Product Requirement: PREQ_006 — Document Generation & HITL Sign-off Flow

## 1. User Story / Description
As a Developer Solo, I need the system to generate
governance documents and present them for my review
and approval in a clear, structured way so that I
can confidently sign off on artifacts knowing exactly
what I am authorizing and what happens next.

## 2. Behavioral & UI Flow Logic

### 2.1 Document Draft Generation
1. When an agent is ready to generate a document,
   it announces what it is about to create:
   * Document type and ID.
   * Which requirement or decision it captures.
   * Estimated scope — single decision or multi-section.
2. Agent generates the document draft and presents
   it to the operator in full — never partially.
3. Agent highlights the key decision points within
   the document so the operator knows what to focus
   their review on.

### 2.2 Operator Review Cycle
1. After presenting the draft the system enters
   review mode and presents three options:
   * **Approve** — document moves to sign-off.
   * **Request Changes** — operator provides specific
     feedback and agent revises.
   * **Reject** — document is discarded and archived
     in rollbacks per PREQ_005 section 2.2. Agent
     restarts from last known good state.
2. If operator requests changes:
   * Agent asks for the specific section or decision
     to change — never rewrites the entire document
     without direction.
   * Agent presents the full revised document with
     modified content visually distinguished from
     unchanged content — never a partial view unless
     the operator explicitly requests section-only
     display.
   * Cycle repeats until operator approves.

### 2.3 HITL Sign-off
1. Upon operator approval the system presents the
   sign-off gate:
   * Document ID and title.
   * Status transition: DRAFT → AUTHORIZED.
   * Consequence: document will be written to disk
     and committed to the repository.
2. Operator provides explicit sign-off — name or
   identifier confirmation.
3. System confirms sign-off, writes document to
   correct governance layer path per BDR_015, and
   notifies operator of successful commit with
   the exact file path.

### 2.4 Document Lineage Transparency
1. If the document being generated supersedes an
   existing document, the system must explicitly
   surface this before sign-off:
   * Which document is being superseded.
   * Where the superseded document will be archived.
   * The link reason connecting old to new.
2. Operator confirms the supersession chain before
   sign-off executes.

### 2.5 Post Sign-off State
1. After successful sign-off the system:
   * Confirms the document is committed.
   * Updates the operator on the SYSTEM_REGISTRY change.
   * Presents the next logical action without
     requiring the operator to ask.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1 (Happy Path — First Draft Approved):**
  Given an agent generates a document draft,
  When the operator reviews and approves it,
  Then sign-off completes and the document is written
  to the correct governance path in a single operation.

* **Scenario 2 (Happy Path — Change Request):**
  Given the operator requests a change,
  When the agent revises,
  Then the full document is presented with modified
  content visually distinguished from unchanged
  content — never a partial view unless explicitly
  requested by the operator.

* **Scenario 3 (Rejection):**
  Given the operator rejects a draft,
  When the system discards it,
  Then the rejected draft is archived in rollbacks
  per PREQ_005 section 2.2 and the agent returns
  to the last known good state without data loss.

* **Scenario 4 (Supersession Transparency):**
  Given a document supersedes an existing one,
  When the sign-off gate is presented,
  Then the supersession chain is explicitly surfaced
  and requires operator confirmation before executing.

* **Scenario 5 (Post Sign-off Navigation):**
  Given a document has been successfully authorized,
  When the operator sees the confirmation,
  Then the system presents the next logical action
  without requiring the operator to ask.

* **Scenario 6 (Error — Wrong Path):**
  Given the Librarian detects the document path
  violates BDR_015 layer structure,
  When the write is attempted,
  Then the system blocks the write, explains the
  violation, and presents the correct path before
  retrying.

* **Scenario 7 (Change Visibility):**
  Given the operator requested a change and the
  agent revised,
  When the revised document is presented,
  Then modified content is visually distinguished
  from unchanged content and the full document
  is visible in context — never partial unless
  operator explicitly requests section-only view.
