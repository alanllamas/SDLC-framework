---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_011
type: PREQ
title: "Brownfield Onboarding & Project Analysis Flow (Path D)"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.13.0 — Brownfield Onboarding"
relationships:
  vertical:
    governing_pdr: PDR_001
    parent_id: null
  horizontal:
    depends_on: []
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

# Product Requirement: PREQ_011 — Brownfield Onboarding & Project Analysis Flow (Path D)

## 1. Intent & Scope
Resolves TQ_015 (GAP_011) — Option Alpha selected. Defines the
operator-facing flow for onboarding the framework onto an
**existing** project, as a new milestone (v0.13.0) at the end
of the v1.0.0 sequence. Governed by PDR_001 (Interaction Model
& Operator Experience Mandate) — this is fundamentally an
extension of the onboarding experience, not a new interaction
paradigm.

## 2. The Flow — "Path D"

### 2.1 Trigger
During the Git Butler Session Gateway (BREQ_017 Step 1 /
ADR_002 Path A), when an existing local repository is detected
WITHOUT a `/governance/` directory, Git Butler offers Path D
as an additional option alongside the existing "initialize
governance plane" choice:

```
This repository doesn't have SDLC governance yet.

A) Initialize governance plane now (start fresh)
B) Analyze this project first, then initialize governance
   with that context (recommended for existing codebases)
```

### 2.2 Project Analysis (if B selected)
Git Butler performs a **lightweight** scan — no LLM required
for the scan itself:
* File tree structure (depth-limited, excluding common
  ignore patterns: node_modules, .git, venv, dist, build).
* Package manifests (package.json, requirements.txt,
  pyproject.toml, go.mod, etc. — whichever present).
* README content (if present).
* Last 20 commit messages (via existing Git adapter).
* Detected primary language(s) and frameworks (inferred
  from manifests/file extensions).

This produces a **Project Analysis Report**
(`governance/business/PROJECT_ANALYSIS_REPORT.md`) — a
structured markdown summary of the above, written via
Librarian per existing write patterns.

### 2.3 Consumption by Business Catalyst
When the Business Catalyst activates for the first time
(first elicitation session per PREQ_001), if
`PROJECT_ANALYSIS_REPORT.md` exists, it is included in the
Business Catalyst's initial context distillation (TDR_003).
The Business Catalyst's opening message acknowledges the
existing project ("I can see this is a [language] project
with [framework]...") rather than asking generic greenfield
discovery questions — the elicitation interview adapts its
opening questions based on what's already known.

### 2.4 Governance Initialization
After analysis (or if Option A chosen directly), governance
plane initialization proceeds exactly as in ADR_002 Path A —
no change to that mechanism. The Project Analysis Report
remains in the repo as a permanent reference document (not
deleted after first use) — future sessions' Business Catalyst
context assembly may reference it for historical "as-is"
context when discussing legacy areas of the codebase.

## 3. Scope Boundaries (v1.0.0)
* **Single-repo only.** Multi-repo workspace analysis is
  explicitly deferred — already tracked in Architecture
  Future Horizon (multi-repo workspace discovery).
* **No code-quality assessment.** The analysis describes
  WHAT exists (structure, stack, recent activity) — it does
  not evaluate code quality, technical debt, or make
  recommendations. That is the Business Catalyst's job during
  elicitation, informed by the report.
* **No dependency on Agent 7 (v0.10.0).** The Project Analysis
  Report is a one-time, lightweight scan at onboarding —
  fundamentally different from Agent 7's ongoing per-ticket
  chronicling. No hard dependency, though both may eventually
  share file-scanning utilities (Tech Lead's discretion during
  v0.13.0 specification).

## 4. Dependencies & Sequencing
* **Depends on v0.1.0** (ADR_002 Path A, Session Gateway
  per BREQ_017) — Path D is an additional branch within the
  existing Path A flow, not a new path requiring a new ADR
  section number. (Tech Lead may choose to formalize this as
  a TDR amendment to the existing bootstrap TDR or as a new
  standalone TDR — left to v0.13.0 specification.)
* **Placement:** v0.13.0, final milestone of the v1.0.0
  sequence, after v0.12.0 (Agent 9 / Compliance Gatekeeper).
  Rationale: v0.13.0 has no dependency on Agents 7-9, but
  placing it last keeps the milestone numbering append-only
  per RULE_001 and reflects that it was the last gap identified.

## 5. Architecture & Technical Specification Status
**DEFERRED to a future Tech Lead cycle (v0.13.0).** This PREQ
establishes the product-level requirement and flow; the
governing ADR extension (or new ADR), TDRs, TREQs, DEV/TEST
tickets are not yet generated. This is a **Skip Declaration**
(reason: DEFERRED) at the Product layer — to be resolved when
the v0.13.0 Tech Lead cycle runs, following the same pattern
as v0.1.0 through v0.12.0.

## 6. Consequences & Impact
* **TQ_015 Resolved** — Option Alpha.
* **GAP_011 status:** PLACED — v0.13.0 inserted into PRD_001
  roadmap as the final v1.0.0 milestone. Technical
  specification remains OPEN pending future Tech Lead cycle.
* **PRD_001 Update Required:** Add v0.13.0 to milestone roadmap.
