# Architecture Layer Index — 01_architecture_baseline.md
> Living document per BDR_016. Maintained exclusively by System Architect via Librarian.

**Last Updated:** 2026-06-03T00:00:00Z
**Layer Owner:** System Architect (Agent 3)

---

## 1. Layer Mission
The Architecture Layer translates authorized product requirements into
concrete technical decisions, system boundaries, and implementation
blueprints.

---

## 2. Active Artifacts Index

| ID | Title | Status |
|:---|:------|:-------|
| ADR_001 | Vendor-Agnostic Architecture via Abstraction Layers & Adapters | AUTHORIZED |
| ADR_002 | Runtime Environment & Repository Bootstrap Protocol | AUTHORIZED |
| ADR_003 | Agent Orchestration Model | AUTHORIZED |
| ADR_004 | Governance State Machine | AUTHORIZED |
| ADR_005 | Project Registry & Operator Profile Implementation | AUTHORIZED |
| AREQ_001 | Offline & Connectivity Resilience Requirements | AUTHORIZED |
| AREQ_002 | Adapter Implementation Pattern Requirements | AUTHORIZED |

---

## 3. Accumulated Intelligence Ledger

* **2026-06-03** — ADR_001 constitutional anchor — adapter pattern mandatory.
* **2026-06-03** — Credential files only explicit exception to BDR_017.
* **2026-06-03** — v1.0 runtime: Linux/WSL only.
* **2026-06-03** — GitHub only Git provider for v1.0.
* **2026-06-03** — ADR_005 resolves Project Registry + Operator Profile
  via unified ~/.sdlc/.
* **2026-06-03** — AREQ_001 defines offline behavior — 10s detection,
  auto state preservation.
* **2026-06-03** — AREQ_002 defines adapter pattern — ABC + Factory +
  AdapterResult, mock adapters mandatory.
* **2026-06-03** — ADR_003 (Agent Orchestration) AUTHORIZED — single
  process sequential, Context Distillation pattern.
* **2026-06-03** — ADR_004 (Governance State Machine) AUTHORIZED —
  two-level file-based state, state.yaml schema defined.
* **2026-06-03** — All 7 deferred PREQs (002,003,005,006,007,008,009)
  unblocked via ADR_003 + ADR_004.

---

## 4. Future Horizon Ledger

* **SDLC Profile Portability** — Export/import ~/.sdlc/profiles/.
* **Multi-account Git support** — Per-repo .butler.env handles in v1.0.
* **Multi-repo workspace discovery** — Broader directory tree scanning.
* **Additional Git providers** — GitLab, Bitbucket via ADR_001.
* **Local LLM support** — Ollama via invoke_llm() adapter.
* **Native Windows support** — Deferred pending demand.
* **Diagram of complete artifact chain with loops** — Requested by
  Architect per BREQ_007.
* **Real-time state sync** — Multi-operator state.yaml conflict
  resolution for v2.0.0.
* **State visualization dashboard** — Connects to Tauri UI v1.1.0.

---

## 5. Ingestion Compliance Declaration — Product → Architecture (2026-06-03)

* **Ingesting Agent:** System Architect
* **Upstream Layer:** Product
* **Artifacts Ingested:** 10 (1 PDR + 9 PREQs)
* **Completeness Check:** PASSED
* **Coverage Check:** PASSED — all skips resolved

### Skip Declarations — Final State
| Artifact ID | Skip Reason | Resolution |
|:------------|:------------|:-----------|
| PDR_001 | GOVERNANCE_POLICY | CONFIRMED — no ADR required |
| PREQ_002, 003, 005, 006, 007, 008, 009 | DEFERRED — ADR_003/004 | RESOLVED — both AUTHORIZED |

### Inherited Skips — Final State
| Artifact ID | Original Skip | Resolution |
|:------------|:--------------|:-----------|
| BDR_012 | CHALLENGED — ADR_003 | RESOLVED — ADR_003 |
| BDR_013 | CHALLENGED — ADR_006 | RESOLVED — ADR_005 |
| BDR_014 | CHALLENGED — ADR_005 | RESOLVED — ADR_005 |
| BDR_015 | GOVERNANCE_POLICY | CONFIRMED — covered by ADR_002 |
| BDR_016 | GOVERNANCE_POLICY | CONFIRMED |
| BDR_017 | GOVERNANCE_POLICY | CONFIRMED — covered by ADR_002 |
| BDR_018 | CHALLENGED — ADR_003 | RESOLVED — ADR_003 |
| BDR_020 | GOVERNANCE_POLICY | CONFIRMED |

* **Conflict Pre-Scan:** PASSED
* **Yellow Items:** NONE — all resolved
* **HITL Sign-off:** Alan Llamas via Console Approval — 2026-06-03

---

## 6. Inherited TQUERYs

| ID | Description | Resolution Owner | Status |
|:---|:------------|:----------------|:-------|
| TQ_002 | Delivery mechanism | System Architect | RESOLVED — ADR_002 |
| TQ_007 | Project Registry location | System Architect | RESOLVED — ADR_005 |
| TQ_012 | ADR_003/004 resolution | System Architect | RESOLVED — ADR_003, ADR_004 |
