# Architecture Layer Index — 01_architecture_baseline.md
> Living document per BDR_016. Maintained exclusively by System Architect via Librarian.

**Last Updated:** 2026-06-03T00:00:00Z
**Layer Owner:** System Architect (Agent 3)

---

## 1. Layer Mission
The Architecture Layer translates authorized product requirements into concrete
technical decisions, system boundaries, and implementation blueprints. It defines
how the platform is built — never what it does for the user or why it exists
commercially.

---

## 2. Active Artifacts Index

| ID | Title | Status |
|:---|:------|:-------|
| ADR_001 | Vendor-Agnostic Architecture via Abstraction Layers & Adapters | AUTHORIZED |
| ADR_002 | Runtime Environment & Repository Bootstrap Protocol | AUTHORIZED |
| ADR_005 | Project Registry & Operator Profile Implementation | AUTHORIZED |
| AREQ_001 | Offline & Connectivity Resilience Requirements | AUTHORIZED |
| AREQ_002 | Adapter Implementation Pattern Requirements | AUTHORIZED |

---

## 3. Accumulated Intelligence Ledger

* **2026-06-03** — ADR_001 constitutional anchor — all integrations via adapter pattern.
* **2026-06-03** — Credential files (.butler.env) only explicit exception to BDR_017.
* **2026-06-03** — v1.0 runtime: Linux/WSL only. Windows native out of scope.
* **2026-06-03** — GitHub only Git provider for v1.0.
* **2026-06-03** — Multiple repos in same directory always require operator selection.
* **2026-06-03** — ADR_003 (Agent Orchestration), ADR_004 (State Machine) deferred to
  Tech Lead — require implementation context.
* **2026-06-03** — ADR_005 resolves both Project Registry (TQ_007) and Operator Profile
  (ADR_006 previously deferred) via unified ~/.sdlc/ directory.
* **2026-06-03** — AREQ_001 defines offline behavior — max 10s failure detection,
  automatic state preservation to ~/.sdlc/.
* **2026-06-03** — AREQ_002 defines adapter pattern — consistent result structure,
  mock adapters mandatory, Adapter Registry in .butler.env.

---

## 4. Future Horizon Ledger
> Informal ideas. Not backlog. Not committed.

* **SDLC Profile Portability** — Export/import ~/.sdlc/profiles/ for multi-machine
  setup or team template sharing. Future: versioned team profile templates.
* **Multi-account Git support** — Each repo's .butler.env handles implicitly in v1.0.
* **Multi-repo workspace discovery** — Broader directory tree scanning.
* **Additional Git providers** — GitLab, Bitbucket, self-hosted via ADR_001.
* **Local LLM support** — Ollama via invoke_llm() adapter.
* **Native Windows support** — Deferred pending demand.
* **Diagram of complete artifact chain with loops** — Visual map of
  BDR→BREQ→PDR→PREQ→AER→ADR→AREQ→TDR→TREQ→DEV_TICKET+TEST_TICKET
  including TQUERY, CR, and BPROP loops. Requested by Architect per BREQ_007.

---

## 5. Ingestion Compliance Declaration — Product → Architecture (2026-06-03)

* **Ingesting Agent:** System Architect
* **Upstream Layer:** Product
* **Artifacts Ingested:** 10 (1 PDR + 9 PREQs)
* **Completeness Check:** PASSED
* **Coverage Check:** PASSED WITH SKIPS

### Skip Declarations (New This Layer)
| Artifact ID | Skip Reason | Reference |
|:------------|:------------|:----------|
| PDR_001 | GOVERNANCE_POLICY — interaction model, no ADR required | null |
| PREQ_002 | DEFERRED — requires ADR_003 (Agent Orchestration) | TQ_007→ADR_003 |
| PREQ_003 | DEFERRED — requires ADR_003 | TQ_007→ADR_003 |
| PREQ_005 | DEFERRED — requires ADR_004 (State Machine) | TQ_007→ADR_004 |
| PREQ_006 | DEFERRED — requires ADR_004 | TQ_007→ADR_004 |
| PREQ_007 | DEFERRED — requires ADR_003 | TQ_007→ADR_003 |
| PREQ_008 | DEFERRED — requires ADR_004 | TQ_007→ADR_004 |
| PREQ_009 | DEFERRED — requires ADR_003 | TQ_007→ADR_003 |

### Inherited Skips (From Business Layer)
| Artifact ID | Original Skip Reason | This Layer Action |
|:------------|:--------------------|:-----------------|
| BDR_012 | GOVERNANCE_POLICY | CHALLENGED — requires ADR_003, DEFERRED to Tech Lead |
| BDR_013 | GOVERNANCE_POLICY | CHALLENGED — requires ADR_006, RESOLVED via ADR_005 |
| BDR_014 | GOVERNANCE_POLICY | CHALLENGED — requires ADR_005, RESOLVED |
| BDR_015 | GOVERNANCE_POLICY | CONFIRMED — covered by ADR_002 |
| BDR_016 | GOVERNANCE_POLICY | CONFIRMED — documental protocol |
| BDR_017 | GOVERNANCE_POLICY | CONFIRMED — partially covered by ADR_002 adapters |
| BDR_018 | GOVERNANCE_POLICY | CHALLENGED — requires ADR_003, DEFERRED to Tech Lead |
| BDR_020 | GOVERNANCE_POLICY | CONFIRMED — process protocol |

* **Conflict Pre-Scan:** PASSED
* **Yellow Items:** PREQ_010 → ADR_005 was in DRAFT. Now RESOLVED — ADR_005 AUTHORIZED.
* **HITL Sign-off:** Alan Llamas via Console Approval — 2026-06-03

---

## 6. Inherited TQUERYs

| ID | Description | Resolution Owner | Status |
|:---|:------------|:----------------|:-------|
| TQ_002 | Delivery mechanism | System Architect | RESOLVED — ADR_002 |
| TQ_007 | Project Registry location | System Architect | RESOLVED — ADR_005 |
