---
extends_schema: "/governance/templates/master_metadata.yaml"
id: ADR_004
type: ADR
title: "Governance State Machine"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_006
    parent_id: null
  horizontal:
    depends_on: [ADR_001, ADR_002, ADR_003, ADR_005]
  cross_plane:
    architecture_adr: [ADR_001, ADR_003]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "System Architect", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Architectural Decision Record: ADR_004 — Governance State Machine

## 1. Context & Problem Statement
The framework must track lifecycle state of every governance
artifact, active session, and planning phase across multiple
projects — persisting between sessions and queryable by any
agent. BDR_005 mandates HITL-authorized transitions. PREQ_001
mandates session state recovery.

## 2. Decision Outcome
* **Chosen Option:** File-based state machine using structured
  YAML in ~/.sdlc/ for operator-level state and
  /governance/bprops/ for project-level state.
* **Status:** AUTHORIZED

## 3. The State Machine Architecture

### 3.1 Two-Level State Model

**Level 1 — Operator State** (~/.sdlc/)
```
~/.sdlc/
├── project_registry.md
├── profiles/[operator_id].yaml
└── projects/[PRJ_ID]/
    ├── context.md
    └── state.yaml
```

**Level 2 — Project State** (/governance/bprops/)
```
/governance/bprops/
├── history/
├── rollbacks/
└── tqueries/
    ├── resolved/
    └── pending/
```

### 3.2 The Project State File (state.yaml)
```yaml
project_state:
  project_id: "PRJ_XXX"
  active_version: "v0.1.0"
  active_milestone: "v0.1.0 — Bootstrap & Session Gateway"
  active_phase: "PHASE_1 | PHASE_2"
  active_layer: "BUSINESS | PRODUCT | ARCHITECTURE | TECHNICAL"
  active_agent: "Business Catalyst | PM Orchestrator | System Architect | Tech Lead"
  hitl_config: "CONFIG_1 | CONFIG_2 | CONFIG_3 | CONFIG_4"
  hitl_operator_id: "[operator_id]"

  session:
    status: "ACTIVE | SUSPENDED | INTERRUPTED"
    last_active: "[ISO-8601]"
    active_document: "[path or null]"
    pending_tqueries: ["TQ_XXX"]
    interrupted_at: "[description or null]"

  phase_gates:
    business_layer_sealed: false
    product_layer_sealed: false
    architecture_layer_sealed: false
    technical_layer_sealed: false
    phase_1_closed: false
    phase_2_closed: false

  ingestion_compliance:
    business_to_product: "PENDING | PASSED | PASSED_WITH_FLAGS | FAILED"
    product_to_architecture: "PENDING | PASSED | PASSED_WITH_FLAGS | FAILED"
    architecture_to_technical: "PENDING | PASSED | PASSED_WITH_FLAGS | FAILED"

  integrity_audit:
    status: "PENDING | IN_PROGRESS | PASSED | FAILED"
    active_gir: "[GIR_ID or null]"
    red_items_count: 0
    yellow_items_count: 0
```

### 3.3 Legal State Transitions

**Phase transitions:**
```
PHASE_1 → PHASE_2: Requires PHASE_1_CLOSED event + HITL sign-off on final GIR.

BUSINESS → PRODUCT: Requires ICD Business→Product PASSED.
PRODUCT → ARCHITECTURE: Requires ICD Product→Architecture PASSED.
ARCHITECTURE → TECHNICAL: Requires ICD Architecture→Technical PASSED.
```

**Session transitions:**
```
ACTIVE → SUSPENDED: Operator explicit suspend command.
ACTIVE → INTERRUPTED: Unexpected disconnection — auto-detected.
SUSPENDED → ACTIVE: Operator resumes — state loaded from state.yaml.
INTERRUPTED → ACTIVE: Operator resumes — interrupted state surfaced.
```

**Document state transitions:**
```
DRAFT → REVIEW: Agent proposes, HITL reviews.
REVIEW → AUTHORIZED: HITL signs off.
AUTHORIZED → DEPRECATED: New superseding document AUTHORIZED.
```

### 3.4 State Write Protocol
* Only Librarian writes to state.yaml per BREQ_010.
* Every write generates Global History Ledger entry per BREQ_013.
* Git Butler stages changes after Librarian writes per BREQ_011.
* ~/.sdlc/ state files written by Librarian but not committed
  to any repo — local operator infrastructure per ADR_005.

### 3.5 State Query Protocol
* Any agent reads state.yaml via read_context() per ADR_001/ADR_005.
* Orchestrator loads state.yaml at session init per ADR_003.
* "Where are we?" answered by reading state.yaml directly —
  no LLM inference required.

## 4. Evaluated Options

### Option A — File-based YAML State Machine (Selected)
* **Pros:** Human-readable, inspectable, persists between sessions,
  Git-compatible, works offline per AREQ_001.
* **Cons:** No real-time consistency for concurrent writes —
  acceptable for v1.0 single-process model.

### Option B — In-memory + Database
* **Pros:** ACID transactions.
* **Cons:** Requires DB setup, over-engineered, not human-readable.

### Option C — Git Commit History as State
* **Pros:** Already versioned.
* **Cons:** Slow to query, not suitable for real-time session state.

## 5. Consequences & Impact
* **Unblocks:** PREQ_002, 003, 005, 006, 007, 008 — all deferred
  PREQs have a state layer to implement against.
* **Downstream — Tech Lead:** Implements state.yaml read/write via
  Librarian. Schema authoritative — fields added via TDR, not removed.
* **Downstream — Orchestrator:** Loads state.yaml before every
  session, updates after every transition.
* **Downstream — Git Butler:** Stages state.yaml changes after
  every Librarian write within project repos.

## 6. Future Horizon Notes
* **Real-time state sync:** Multi-operator teams (v2.0.0) require
  conflict resolution for simultaneous state.yaml writes —
  dedicated ADR.
* **State visualization:** Dashboard showing complete state of
  active projects — connects to Tauri UI planned for v1.1.0.
