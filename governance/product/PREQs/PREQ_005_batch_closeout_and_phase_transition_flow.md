---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_005
type: PREQ
title: "Batch Closeout & Phase Transition Flow"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_004, BREQ_003, BREQ_014, BDR_016]
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

# Product Requirement: PREQ_005 — Batch Closeout & Phase Transition Flow

## 1. User Story / Description
As a Developer Solo, I need the system to clearly signal
when a planning phase is complete, confirm that nothing
has been missed, and guide me cleanly into the next phase
so that I never accidentally leave loose ends behind or
jump ahead before the current layer is properly sealed.

## 2. Behavioral & UI Flow Logic

### 2.1 Batch Closeout Sequence
1. When an agent completes its last document in a phase,
   it announces the completion:
   * What was produced in this phase — a scannable
     list of all authorized documents.
   * A confirmation that all required artifacts are
     present and authorized before proceeding.
2. Agent runs the closeout interrogation per BREQ_003:
   * "Are there any additional items required for
     this planning batch?"
   * "Do you have any ideas or desires to capture
     for the Future Horizon before we close?"
3. Routing per operator response:
   * **Additional items:** Agent initializes a new
     document generation cycle for the new item
     before closeout resumes.
   * **Future Horizon items:** Agent captures the
     input into the Future Horizon Ledger of the
     current layer's index document per BDR_016 —
     never in another layer's document. System
     confirms the item was saved before continuing.
   * **Nothing to add:** Agent proceeds to phase
     transition gate.

### 2.2 Draft Retention Protocol
* In-progress drafts that have not been authorized
  are never automatically archived to rollbacks.
* All drafts persist in their active proposal/[DOC_ID]
  Git branch until the operator explicitly rejects them.
* Upon explicit rejection by the operator, the draft
  is archived to /governance/bprops/rollbacks/ and
  the branch is closed.
* If the operator suspends the session without rejecting
  a draft, the draft remains on its branch intact and
  is surfaced upon session resume per Scenario 5.

### 2.3 Session Persistence & Recovery
* Every significant agent-operator exchange triggers
  an automatic micro-commit to the active
  proposal/[DOC_ID] branch — not just on sign-off.
* This ensures that if a session is interrupted
  unexpectedly, the maximum recoverable state is
  the last significant exchange — never the entire
  session.
* Upon session resume, the Interrupted Session
  Recovery flow per PREQ_001 Scenario 3 surfaces
  the last persisted state automatically.

### 2.4 Phase Seal Confirmation
1. Before transitioning to the next phase the system
   presents a Phase Seal Summary:
   * Phase name and agent role completing.
   * Complete list of authorized artifacts with
     their file paths.
   * Any pending TQUERYs being carried forward
     to the next phase with their resolution owners.
   * Future Horizon items captured this session.
2. Operator explicitly confirms the seal.
3. Upon confirmation the system:
   * Marks the phase as sealed in the session state.
   * Updates SYSTEM_REGISTRY with all new authorized
     artifacts.
   * Updates the current layer's index document
     Active Artifacts Index per BDR_016.

### 2.5 Phase Transition
1. System announces the incoming phase and agent role:
   * What the next phase will produce.
   * What the operator can expect from the next agent.
   * Any TQUERYs inherited from the previous phase.
2. Operator confirms readiness to begin the next phase.
3. System activates the next agent role per PREQ_002.
4. Incoming agent initializes its Layer Index Document
   if this is the first session for that layer per BDR_016.

### 2.6 Session Suspension
1. If the operator needs to stop before a phase is
   complete, they may suspend the session at any point.
2. System presents a suspension summary:
   * What was completed and authorized this session.
   * What remains incomplete and on which branches.
   * Where the session will resume next time.
3. All in-progress drafts remain on their proposal
   branches per section 2.2 — nothing is discarded
   on suspension.
4. System confirms the suspension state before closing.

### 2.7 Future Horizon Ledger Query
1. At any point the operator may request to see all
   items in the current layer's Future Horizon Ledger.
2. System presents the full ledger in a scannable
   format — item description and capture timestamp —
   with a clear reminder that these are informal
   ideas, not committed backlog.

## 3. Functional Acceptance Criteria (Product DoD)

* **Scenario 1 (Happy Path — Clean Closeout):**
  Given all phase documents are authorized,
  When the closeout interrogation returns no additional
  items,
  Then the Phase Seal Summary is presented and the
  operator can confirm the seal in a single response.

* **Scenario 2 (Additional Item Added):**
  Given the operator identifies an additional item
  during closeout,
  When the agent initializes a new generation cycle,
  Then the closeout sequence resumes only after the
  new item is authorized — never skipped.

* **Scenario 3 (Future Horizon Capture):**
  Given the operator provides a Future Horizon item,
  When the agent captures it,
  Then a confirmation that the item was saved to
  the current layer's index document is presented
  before the closeout continues. Zero cross-layer
  document modifications permitted.

* **Scenario 4 (Pending TQUERYs Carried Forward):**
  Given TQUERYs remain unresolved at phase closeout,
  When the Phase Seal Summary is presented,
  Then all pending TQUERYs are listed with their
  resolution owners so the operator is fully aware
  of what is being inherited by the next phase.

* **Scenario 5 (Session Suspension & Resume):**
  Given the operator suspends the session mid-phase,
  When the operator resumes in a new session,
  Then all in-progress drafts are surfaced from
  their proposal branches with zero data loss.

* **Scenario 6 (Unexpected Interruption Recovery):**
  Given the session is interrupted unexpectedly,
  When the operator starts a new session,
  Then the system recovers to the last micro-commit
  state and presents the interrupted state clearly
  before any new action is taken.

* **Scenario 7 (Explicit Draft Rejection):**
  Given the operator explicitly rejects an in-progress
  draft,
  When the rejection is confirmed,
  Then and only then the draft is archived to
  rollbacks and its proposal branch is closed.

* **Scenario 8 (Error — Unsigned Artifacts at Seal):**
  Given one or more documents remain in DRAFT status
  at closeout time,
  When the operator attempts to confirm the phase seal,
  Then the system blocks the seal, lists the unsigned
  artifacts, and presents options to authorize or
  explicitly reject each before proceeding.
