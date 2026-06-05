# Business Intent & Core Constraints Baseline

## 1. Overarching Commercial Mission
To construct an automated, state-driven Multi-Agent SDLC Governance Platform that enforces absolute communication fidelity, safeguards architectural decisions, and captures human developer desires without omission.

## 2. Operator Vision & Desires Feedback Ledger (Advisory to Agent 3)
* **App Delivery Framework Option:** Tauri (Targeting cross-platform compilation for Web and Desktop options).
* **Notification Engine Layer Options:** Slack or WhatsApp (Targeting immediate, rapid-fire operational alerting to human operators).
* **Architectural Mandate:** Every integrated vendor or platform option must strictly pass evaluation through Framework ADR-001 (The Native Connector Policy) to bypass generic abstractions.

## 3. System Anti-Goals & Strict Boundaries
* [AG-001] The platform must never allow silent feature creep or implicit scope additions.
* [AG-002] Development or execution tasks (`TSK`) cannot be automatically generated until the requirements layer is decoupled and sealed.

## 4. Operator Profile Registry
> Established during PM Orchestrator cycle — 2026-06-03.
> Resolves TQ_001 raised by PM Orchestrator.

The framework recognizes the following canonical operator
profiles. Each profile defines the primary user archetype,
their assumed technical context, and the implementation
phase in which their experience is optimized.

### Primary — Developer Solo (v1.0)
* **Config:** Config 2 — Single Human Operator per BDR_012.
* **Assumptions:** Technically capable, comfortable with Git
  and markdown, familiar with software development lifecycle
  concepts. Needs governance structure and process discipline
  — not hand-holding on technical basics.
* **Framework Interaction:** Operates all HITL roles
  sequentially within the same session. Subject to
  cooling-off gates per BREQ_014.
* **Implementation Phase:** v1.0 — Primary target.
  All current planning is scoped to this profile.

### Alternate A — Small Team / Team Lead (v1.1)
* **Config:** Config 1 or Config 3 per BDR_012.
* **Assumptions:** 2-5 operators with mixed technical levels.
  Needs coordination mechanics, conflict resolution between
  operators, and role clarity across multiple humans.
* **Framework Interaction:** Multiple humans occupy distinct
  HITL roles simultaneously or with partial agent delegation.
* **Implementation Phase:** v1.1

### Alternate B — Solo Operator + Agent HITL (v1.1)
* **Config:** Config 3 per BDR_012.
* **Assumptions:** Experienced Developer Solo who wants to
  accelerate by delegating specific HITL roles to agents
  while retaining strategic control.
* **Framework Interaction:** Human retains at minimum one
  HITL seat. Agent HITLs operate at configured capability
  level per BREQ_015.
* **Implementation Phase:** v1.1

### Alternate C — Incomplete Team with Agent Coverage (v2.0)
* **Config:** Config 4 per BDR_012.
* **Assumptions:** Team missing one or more members requiring
  temporary agent coverage of vacant roles with explicit
  time-boxing and authority ceiling.
* **Framework Interaction:** Agent HITLs cover vacant roles
  autonomously with Level 3 capability, subject to
  retroactive human review per BREQ_014 section 2.4.
* **Implementation Phase:** v2.0

## 5. Business Requirements Index
* [REQ_001_technical_distinction](/governance/business/BREQs/BREQ_001_technical_distinction.md) — Technical Input Distinction Engine.
* [REQ_002_conflict_management](/governance/business/BREQs/BREQ_002_conflict_management.md) — Scope Conflict, Universal BDR, & Pattern-Matching Engine.
* [REQ_003_closeout_loop](/governance/business/BREQs/BREQ_003_closeout_loop.md) — Batch Closeout & Horizon Harvesting Sequence.

## 6. Future Product Horizon
> Items captured here are informal — not backlog, not committed,
> not prioritized. Activated only by explicit HITL decision per BDR_016.

* **Meeting Intelligence Assistant** — The framework extends
  its governance capabilities to the full meeting lifecycle:
  automatic capture of decisions made in meetings, generation
  of structured minutes with traceability to affected governance
  artifacts, detection of commitments and actionable tracking,
  and status reports ready to present to stakeholders — all
  without additional administrative overhead. Surfaces naturally
  as a complement to passive listening and capture capabilities.

* **Multi-organization operator support** — A developer operating
  across multiple organizations or client accounts needs the
  framework to maintain separate operator contexts per organization
  while sharing a single local installation.

* **Git Butler Phase 2 Developer Assistant** — Extend the Git
  Butler mandate beyond governance operations to serve as the
  primary Git interface for developers during Phase 2. Manages
  branch creation per ticket, enforces structured commit messages
  with governance traceability, handles PR creation with full
  metadata, and prevents incorrect Git operations — removing Git
  administrative overhead from developers and centralizing version
  control in a single auditable service.
