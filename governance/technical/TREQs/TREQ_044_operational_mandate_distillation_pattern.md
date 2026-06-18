---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_044
type: TREQ
title: "Operational Mandate Distillation Pattern"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.2.0 — Agent Role Activation (retrofit)"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_003
  horizontal:
    depends_on: [TREQ_011, TREQ_035, TREQ_037, TREQ_040]
  cross_plane:
    architecture_adr: ADR_003
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_044 — Operational Mandate Distillation Pattern

## 1. Intent & Scope
Defines the `OPERATIONAL_MANDATE` constant pattern per AREQ_003
and identifies the 7 existing agent modules requiring
retrofit distillation.

## 2. Technical Constraints

### 2.1 Module Pattern
```python
# Example: /src/agents/technical_process_chronicler.py

AGENT_ID = "Technical Process Chronicler"

# Traceability only — NOT read by the LLM at runtime.
# Source: this repo's governance/business/BREQs/BREQ_020_*.md
GOVERNING_BREQ = "BREQ_020"

OPERATIONAL_MANDATE = """
Your job is to analyze the code changes made for a completed
development ticket and write a clear, concise technical summary
describing what changed and how it works. This summary is for
engineers who join the project later and need to understand the
codebase's evolution without re-reading every diff.

For each completed ticket:
1. Review the code delta (the diff) you are given.
2. Identify what files changed and why, in plain technical language.
3. Note any implementation decisions visible in the code that
   weren't explicit in the ticket description.
4. Write 2-4 paragraphs — factual, no marketing language, no
   speculation about intent beyond what the code shows.

You do not approve or reject the change — that already happened.
Your only output is the written summary.
"""

def get_system_prompt(state: "ProjectState", distillation: dict) -> str:
    return f"""
You are the {AGENT_ID} for this project.

{OPERATIONAL_MANDATE}

CURRENT STATE:
- Project: {state.project_id}
- Completed ticket: {state.session.active_ticket}

RECENT CONTEXT:
{distillation['session_summary']}
"""
```

* `OPERATIONAL_MANDATE` is the distilled, second-person,
  imperative translation of the governing artifact's
  operative sections — NOT a copy-paste of the BREQ's YAML
  front-matter, "Evaluated Alternatives", or "QA Metrics"
  sections (those are planning-time/HITL-review content,
  not agent-operating instructions).
* `GOVERNING_BREQ` remains for THIS repo's own traceability
  (Agent 7/9 during framework development) — a code comment,
  never interpolated into the returned prompt string.

### 2.2 Retrofit Scope — 7 Agent Modules
| Module (existing, AUTHORIZED) | Governing Artifact | Source TREQ |
|:-------------------------------|:--------------------|:------------|
| `/src/agents/business_catalyst.py` | BDR_005 (Human-Centric Expert Assistant Model) + BREQ_001 (Technical Input Distinction) | TREQ_011 |
| `/src/agents/pm_orchestrator.py` | BREQ_004 (PM Orchestrator Mandate) | TREQ_011 |
| `/src/agents/system_architect.py` | BREQ_007 (System Architect Mandate) | TREQ_011 |
| `/src/agents/tech_lead.py` | BREQ_008 (Tech Lead Mandate) | TREQ_011 |
| `/src/agents/technical_process_chronicler.py` | BREQ_020 (Agent 7 Mandate) | TREQ_035 |
| `/src/agents/product_translator.py` | BREQ_021 (Agent 8 Mandate) | TREQ_037 |
| `/src/agents/compliance_gatekeeper.py` | BREQ_022 (Agent 9 Mandate) | TREQ_040 |

For `business_catalyst.py`, the distillation draws from both
BDR_005 (the foundational interaction model — human-centric,
non-replacing, expert-assistant framing) and BREQ_001 (how to
distinguish technical constraints from preferences during
elicitation) — Business Catalyst has no single dedicated
mandate BREQ the way Agents 2/3/4/7/8/9 do.

### 2.3 Git Butler & Librarian — Out of Scope
Agents 5 (Git Butler) and 6 (Librarian) are deterministic
services per their respective TDRs — they do not call
`invoke_llm()` for their core operations (session gateway,
file I/O, registry access are pure filesystem/Git operations).
Git Butler's "Lateral Credential Assistance" (BREQ_017 section 4)
uses static instructional templates, not LLM-generated prose —
no `get_system_prompt()` exists for these two, so AREQ_003
does not apply to them.

## 3. Verification & QA Criteria
* Each of the 7 modules defines `OPERATIONAL_MANDATE` as a
  module-level string constant.
* `get_system_prompt()` for each module includes
  `OPERATIONAL_MANDATE` verbatim in its returned string.
* Regex scan of all 7 modules' `get_system_prompt()` return
  values for `BREQ_\d{3}|BDR_\d{3}|PREQ_\d{3}|ADR_\d{3}|TREQ_\d{3}`
  outside of `GOVERNING_BREQ`-style comments returns zero matches.
* Test harness (AREQ_003 Metric 3) instantiates all 7 modules
  with this repo's `/governance/business/`, `/governance/product/`,
  `/governance/architecture/`, `/governance/technical/`
  directories removed — `get_system_prompt()` succeeds for all.
