---
extends_schema: "/governance/templates/master_metadata.yaml"
id: ADR_003
type: ADR
title: "Agent Orchestration Model"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_006
    parent_id: null
  horizontal:
    depends_on: [ADR_001, ADR_002, ADR_005]
  cross_plane:
    architecture_adr: [ADR_001]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "System Architect", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Architectural Decision Record: ADR_003 — Agent Orchestration Model

## 1. Context & Problem Statement
The framework requires multiple agent profiles (Business Catalyst,
PM Orchestrator, System Architect, Tech Lead, Git Butler, Librarian)
to operate within a single CLI process. BDR_005 mandates strict 1:1
human-centric interaction. The orchestration model must enforce this
while maintaining context continuity across agent transitions and
LLM API calls.

## 2. Decision Outcome
* **Chosen Option:** Single-process sequential orchestration with
  stateful context management via ~/.sdlc/ and in-memory session state.
* **Status:** AUTHORIZED

## 3. The Orchestration Model

### 3.1 Single Active Agent Per Session
* At any given moment exactly one agent profile is active.
* Agent identity declared in session state before any LLM call.
* Agent switching follows cooling-off gate protocol per BREQ_014.

### 3.2 Agent Identity via System Prompt
* Each agent is a distinct system prompt injected into invoke_llm().
* System prompt contains role definition, operational mandate
  (referencing its BREQ), and current session context.
* All LLM calls route through invoke_llm() per ADR_001.

### 3.3 Context Assembly Per Call
```
Context = System Prompt (Agent Identity)
        + Session State (current phase, active document, pending TQUERYs)
        + Conversation History (recent exchanges, capped at token limit)
        + Active Artifact (current document being worked on)
        + Operator Input (current message)
```
* Session State persists to ~/.sdlc/projects/[PRJ_ID]/context.md
  after every significant exchange per AREQ_001.
* Conversation History capped — Tech Lead defines specific token
  cap as TDR.
* Active Artifact loaded from filesystem via Librarian.
* Agent may consult full history in ~/.sdlc/ on demand — never
  loaded automatically.

### 3.4 The Orchestrator Component
* Reads active agent identity from session state.
* Assembles context package per section 3.3.
* Calls invoke_llm() with assembled context.
* Routes response to appropriate handler.
* Writes state changes via Librarian.
* Never exposes LLM internals to operator.

### 3.5 Git Butler & Librarian as System Services
* Librarian: File I/O service with schema validation. Local module,
  no LLM calls.
* Git Butler: Git operations service via Git adapter per ADR_001.
* Both always available regardless of LLM API connectivity per AREQ_001.

## 4. Evaluated Options

### Option A — Single Process Sequential (Selected)
* **Pros:** Simple, predictable, enforces BDR_005 1:1 model naturally.
* **Cons:** No parallel agent execution — acceptable for v1.0.

### Option B — Multi-Process with Message Queue
* **Pros:** Enables future parallel execution.
* **Cons:** Over-engineered for v1.0, complex state synchronization.

## 5. Consequences & Impact
* **Unblocks:** PREQ_002, 003, 005, 006, 007, 008, 009 — all deferred
  PREQs now have orchestration model.
* **Unblocks:** BDR_012, 018 — config switching and audit dossier
  compilation now have implementation path.
* **Downstream — Tech Lead:** Implements Orchestrator as core module.
  Context assembly strategy defined as TDR.
* **Future:** Multi-process deferred to Config 1 requirements.
