---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PRD_001
type: PRD
title: "Product Roadmap: SDLC Governance Framework"
status: DRAFT
relationships:
  vertical:
    governing_bdr: BDR_002
    parent_id: null
  horizontal:
    depends_on: [BREQ_018]
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

# LIVING DOCUMENT — never sealed, never AUTHORIZED.
# Updated at every phase closeout per BREQ_003.
---

# Product Roadmap: SDLC Governance Framework

## 1. Vision Anchor
An automated, state-driven Multi-Agent SDLC Governance Platform
that enforces absolute communication fidelity, safeguards
architectural decisions, and captures human developer desires
without omission. The platform translates any business idea into
a fully traceable, executable development plan — and accompanies
its execution through completion.

---

## 2. Versioning Semantics
> Default Semantic Versioning pattern adopted as-is per BREQ_018.

| Version Type | Trigger | Planning Cycles Required |
|:-------------|:--------|:------------------------|
| **Major (X.0.0)** | New primary operator profile or paradigm shift. Anti-Goal modified. | Business + Product + Arch + Tech |
| **Minor (X.Y.0)** | New capability within same primary operator. New HITL config, agent layer, or integration. | Product + Arch + Tech |
| **Patch (X.Y.Z)** | UX improvement, bug fix, performance — no new scope. | Tech (with Arch review) |

---

## 3. Version Registry

### Version: v1.0.0 — SDLC Framework Complete
* **Primary User Profile:** Developer Solo (Config 2)
* **Secondary User Profiles:** NONE
* **Scope Boundary:**
    * **Does:**
        * Full Phase 1 planning pipeline (Business → Product → Architecture → Tech Lead)
        * Full Phase 2 development execution support — Git Butler dev mode
        * Agent 7 (Technical Process Chronicler) operational
        * Agent 8 (Product Delivery Translator) operational
        * Agent 9 (Lifecycle Compliance Gatekeeper) operational
        * CLI conversational interface on Linux/WSL
        * GitHub adapter, Anthropic Claude API adapter
        * Session persistence via ~/.sdlc/
        * Project Registry and Operator Profile management
        * Full TQUERY, CIR, BPROP pipeline
        * Phase 1 Governance Integrity Audit (GIR)
        * Ingestion Compliance Protocol between all layers
    * **Does Not:**
        * Multi-operator team support (Config 1/4)
        * Agent HITL delegation (Config 3)
        * Tauri UI
        * Notification engine
        * Enterprise multi-organization support
* **Planning Status:** IN_PROGRESS
* **Dependencies:** NONE — first version
* **Governing PDRs:** PDR_001
* **Last Updated:** 2026-06-03

#### Milestones
| Milestone ID | Name | Assigned PREQs | Status | MVP? |
|:-------------|:-----|:---------------|:-------|:-----|
| v0.1.0 | Bootstrap & Session Gateway | PREQ_001 | IN_PROGRESS | |
| v0.2.0 | Agent Role Activation & Transitions | PREQ_002 | PLANNED | |
| v0.3.0 | TQUERY Flow | PREQ_003 | PLANNED | |
| v0.4.0 | Document Generation & Sign-off | PREQ_006 | PLANNED | |
| v0.5.0 | Technical Input & Scope Clarification | PREQ_009 | PLANNED | |
| v0.6.0 | Conflict Intelligence & History | PREQ_007, PREQ_008 | PLANNED | |
| v0.7.0 | Innovation Proposal & Project Registry | PREQ_010 | PLANNED | |
| v0.8.0 | Batch Closeout, Phase Transitions & Integrity Audit | PREQ_005, GIR, ICD flows | PLANNED | ⭐ MVP |
| v0.9.0 | Phase 2 Execution — Developer Workflow | Git Butler dev mode, Dev feedback protocol | PLANNED | |
| v0.10.0 | Agent 7 — Technical Process Chronicler | Code annotation, tech doc generation | PLANNED | |
| v0.11.0 | Agent 8 — Product Delivery Translator | Release notes, user manual delta | PLANNED | |
| v0.12.0 | Agent 9 — Compliance Gatekeeper | Merge gates, compliance planes | PLANNED | |
| v1.0.0 | Framework Complete | All above | PLANNED | |

---

### Version: v1.1.0 — Agent HITL & Extended UI
* **Primary User Profile:** Developer Solo (Config 2)
* **Secondary User Profiles:** Solo Operator + Agent HITL (Config 3)
* **Scope Boundary:**
    * **Does:** Everything in v1.0.0, Agent HITL delegation per BDR_013,
      Capability levels, Adaptive Capability Assessment, Pair Mode,
      Tauri UI, Notification engine
    * **Does Not:** Full team multi-operator support, Config 4
* **Planning Status:** PLANNED
* **Dependencies:** v1.0.0 COMPLETE
* **Governing PDRs:** TBD
* **Last Updated:** 2026-06-03

#### Milestones
| Milestone ID | Name | Assigned PREQs | Status | MVP? |
|:-------------|:-----|:---------------|:-------|:-----|
| v1.1.0 | Agent HITL + Extended UI | TBD | PLANNED | |

---

### Version: v2.0.0 — Full Human Team
* **Primary User Profile:** Small Team / Team Lead (Config 1)
* **Secondary User Profiles:** Incomplete Team with Agent Coverage (Config 4)
* **Scope Boundary:**
    * **Does:** Everything in v1.1.0, Multi-operator support,
      Config 1/4, Shared Project Registry, Team profile templates
    * **Does Not:** Enterprise multi-org, multi-tenant licensing
* **Planning Status:** PLANNED
* **Dependencies:** v1.1.0 COMPLETE
* **Governing PDRs:** TBD
* **Last Updated:** 2026-06-03

#### Milestones
| Milestone ID | Name | Assigned PREQs | Status | MVP? |
|:-------------|:-----|:---------------|:-------|:-----|
| v2.0.0 | Full Team Support | TBD | PLANNED | |

---

### Version: v3.0.0 — Enterprise & Licensing
* **Primary User Profile:** Organization / Multi-team
* **Scope Boundary:**
    * **Does:** Everything in v2.0.0, Multi-org support,
      Enterprise version control, Licensable multi-tenant,
      Meeting Intelligence Assistant, SDLC Profile Portability
    * **Does Not:** TBD
* **Planning Status:** PLANNED
* **Dependencies:** v2.0.0 COMPLETE
* **Governing PDRs:** TBD
* **Last Updated:** 2026-06-03

#### Milestones
| Milestone ID | Name | Assigned PREQs | Status | MVP? |
|:-------------|:-----|:---------------|:-------|:-----|
| v3.0.0 | Enterprise | TBD | PLANNED | |

---

## 4. Gap Registry

| Gap ID | Description | Blocks Version | Owner Layer | Status |
|:-------|:------------|:--------------|:------------|:-------|
| GAP_001 | ADR_003 implementation details | v0.1.0 | Architecture | RESOLVED — ADR_003 |
| GAP_002 | ADR_004 not yet defined | v0.3.0 | Architecture | RESOLVED — ADR_004 |
| GAP_003 | Extended operator flow PREQs not planned | v1.1.0 | Product | OPEN |
| GAP_004 | Tauri UI architecture not defined | v1.1.0 | Architecture | OPEN |
| GAP_005 | Notification engine adapter not designed | v1.1.0 | Architecture | OPEN |
| GAP_006 | Phase 2 execution support planning not started | v0.9.0 | Product + Architecture | OPEN |
| GAP_007 | Documentation agents (7,8,9) operational planning not started | v0.10.0 | Product + Architecture | OPEN |
| GAP_008 | Multi-organization support architecture not defined | v3.0.0 | Architecture | OPEN |
| GAP_009 | Developer Feedback Protocol not defined | v0.9.0 | Technical | OPEN |
| GAP_010 | External feedback capture for MVP testing | v0.8.0 | Technical | RESOLVED — TDR_005 |

---

## 5. Release History
> Updated by Git Butler when a milestone is tagged in the repository.

| Milestone | Release Date | Git Tag | Notes |
|:----------|:------------|:--------|:------|
| — | — | — | No releases yet |
