# SDLC Governance Framework

An automated, state-driven Multi-Agent SDLC Governance Platform that enforces absolute communication fidelity, safeguards architectural decisions, and captures human developer desires without omission.

## Repository Structure

```
/
├── governance/
│   ├── business/           # Business layer — BDRs + BREQs
│   │   ├── BDRs/           # Business Decision Records
│   │   ├── BREQs/          # Business Requirements
│   │   └── 01_business_request.md
│   ├── product/            # Product layer
│   │   ├── PDRs/
│   │   └── PREQs/
│   ├── architecture/       # Architecture layer
│   │   ├── ADRs/
│   │   ├── AREQs/
│   │   └── TDRs/
│   ├── technical/          # Technical layer
│   │   ├── TREQs/
│   │   └── schemas/
│   ├── templates/          # Master schemas and scaffolding
│   └── bprops/
│       ├── history/        # Global audit ledgers
│       └── rollbacks/      # Archived stale versions
├── src/                    # Development plane
├── SYSTEM_REGISTRY.md
└── .gitignore
```

## Agent Roster

| Agent | Role | Mandate |
|:------|:-----|:--------|
| Agent 1 | Business Catalyst | Strategy & Governance |
| Agent 2 | PM Orchestrator | Requirements & Organization |
| Agent 3 | System Architect | Design & Integrity |
| Agent 4 | Tech Lead | Implementation & Execution |
| Agent 5 | Git Butler | Operations & Traceability |
| Agent 6 | Librarian | File I/O Security Kernel |
| Agent 7 | Technical Process Chronicler | Code Documentation |
| Agent 8 | Product Delivery Translator | Release Documentation |
| Agent 9 | Lifecycle Compliance Gatekeeper | QA & Compliance |

## Core Governance Principles

- **Immutability:** No artifact is ever modified. Evolution occurs through supersession chains.
- **Traceability:** Every artifact links back to its business intent origin.
- **Human Sovereignty:** All decisions originate from or are explicitly approved by the HITL.
- **Intelligence-First:** No assumptions. Knowledge gaps surface as TQUERYs.
- **Governance-as-Code:** All artifacts live in the same repository as the source code.

## Getting Started

See `SYSTEM_REGISTRY.md` for a complete index of all active governance artifacts.
