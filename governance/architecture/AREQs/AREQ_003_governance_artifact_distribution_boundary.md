---
extends_schema: "/governance/templates/master_metadata.yaml"
id: AREQ_003
type: AREQ
title: "Governance Artifact Distribution Boundary & Agent Prompt Self-Containment"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_015
    parent_id: ADR_002
  horizontal:
    depends_on: [ADR_001, ADR_002, AREQ_002]
  cross_plane:
    architecture_adr: [ADR_001, ADR_002]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "System Architect", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Architectural Requirement: AREQ_003 — Governance Artifact Distribution Boundary & Agent Prompt Self-Containment

## 1. Intent & Scope
* **Description:** Establishes a hard boundary between this
  repository's OWN `/governance/` planning artifacts (the
  documents that governed the framework's construction —
  BDRs, BREQs, PDRs, PREQs, ADRs, AREQs, TDRs, TREQs, tickets)
  and what is actually shipped in the distributable package.
  Requires every LLM-facing agent prompt to be **self-contained**
  — operable with zero runtime filesystem access to this
  repo's `/governance/` planning documents.
* **Business/Product Rationale:** The framework's own `/governance/`
  is dogfooding artifact — proof of HOW the framework was built,
  consumed by humans (HITL review) and by this framework's own
  Agent 7/8/9 during ITS development. End users running the
  distributed framework on THEIR projects never see or need
  this repo's BDR_020, BREQ_022, etc. If an agent's system
  prompt's only description of its operating rules is "see
  BREQ_020", the LLM receiving that prompt at runtime has no
  way to resolve it — a dangling reference that silently
  degrades agent behavior to whatever the base model guesses.

## 2. Technical Constraints & Bounds

### 2.1 What Ships vs. What Doesn't
* **Ships (part of `/src/` distributable package):**
    * All Python source — agents, adapters, Orchestrator, Librarian.
    * `/governance/templates/` — scaffolding templates
      (BDR_XXX.md, BREQ_XXX.md, TQUERY_metadata.yaml, etc.).
      These are copied into END-USERS' projects by Git Butler
      during bootstrap (ADR_002 Path C step 5) to create THEIR
      `/governance/` structure — templates are product, not
      meta-governance.
* **Does NOT ship:**
    * This repo's own filled-in `/governance/business/`,
      `/governance/product/`, `/governance/architecture/`,
      `/governance/technical/` directories — these are the
      framework's OWN planning history, relevant only to this
      project's development and to Anthropic/Alan's own
      Agent 7/8/9 runs during framework development.

### 2.2 Agent Prompt Self-Containment Rule
Every agent module following the TREQ_011 pattern
(`get_system_prompt(state, distillation) -> str`) MUST:
* Include an `OPERATIONAL_MANDATE` string constant — a
  natural-language, LLM-actionable translation of the agent's
  governing artifact's operative rules, fully contained within
  the module's own source code.
* The returned prompt string MUST embed `OPERATIONAL_MANDATE`
  directly — never merely cite a governance document ID as
  the sole description of the agent's behavior.
* `GOVERNING_BREQ = "BREQ_XXX"` style constants MAY remain as
  module-level metadata (docstrings/comments) for THIS repo's
  own traceability (Agent 7 chronicling, Agent 9 audits during
  framework development) — but are NEVER read by, nor required
  by, the running LLM.

### 2.3 Violation Detection
Agent 9 (Lifecycle Compliance Gatekeeper, TREQ_040) extends its
AREQ_002 violation-detection checks to flag:
* Any `get_system_prompt()` whose returned string contains a
  bare governance-artifact-ID pattern (e.g., `BREQ_\d{3}`,
  `BDR_\d{3}`) WITHOUT an accompanying `OPERATIONAL_MANDATE`
  constant defined in the same module.

## 3. HITL Intelligence & Review Loop
* Distillation quality (how faithfully `OPERATIONAL_MANDATE`
  represents its source BREQ/PREQ) is a judgment call made
  during DEV_TICKET implementation — reviewed by HITL via
  TDR_008 sign-off of the resulting chronicle entry (Agent 7),
  same as any other code change.

## 4. Verification & QA Criteria
* Metric 1: 100% of agent modules using `get_system_prompt()`
  define an `OPERATIONAL_MANDATE` constant.
* Metric 2: 0% of `get_system_prompt()` return values contain
  a governance-artifact-ID as the sole description of any
  operating rule.
* Metric 3: A test harness can construct and run any agent
  module with `/governance/business/`, `/governance/product/`,
  `/governance/architecture/`, `/governance/technical/` (this
  repo's own planning dirs) entirely absent from the filesystem
  — agent prompt generation must not error or degrade.
* Metric 4: `/governance/templates/` is the only `/governance/`
  subtree referenced by any runtime code path.

## 5. Future Horizon Notes
* **Mandate versioning:** If a governing BREQ is superseded in
  a future framework version, `OPERATIONAL_MANDATE` constants
  must be updated in the same DEV_TICKET cycle — Agent 9's
  mini coverage check (TREQ_040) could be extended to flag
  agent modules whose `GOVERNING_BREQ` comment references a
  superseded document, as a reminder to re-distill.
