---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_033
type: TREQ
title: "Developer Log Classification Prompt & Routing"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.9.0 — Phase 2 Execution: Developer Workflow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_017
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_033 — Developer Log Classification Prompt & Routing

## 1. Intent & Scope
Defines the `/log` command, its classification prompt, and
routing to existing pipelines per TDR_017.

## 2. Technical Constraints

### Command Recognition
* `/log` recognized as explicit command. Additionally, agent
  system prompts (TREQ_011) instruct: if operator describes
  something during Execution Mode that sounds like a discovery
  rather than a task update (e.g., "I noticed...", "this is
  broken...", "it would be nice if..."), agent emits
  `<<DEV_LOG_SUGGESTED>>` directive — Orchestrator offers `/log`
  as a suggestion, operator confirms or dismisses.

### Classification Prompt
```python
def render_log_classification() -> str:
    return """
What did you find?

A) Blocker — I can't proceed without resolving this
B) Bug — something is broken (not blocking current ticket)
C) Technical Debt — works, but should be improved later
D) Idea — innovation or improvement worth considering
E) Just a note — capture for later, no action needed

Reply A-E.
"""
```

### Routing
```python
def route_dev_log(
    choice: Literal["A","B","C","D","E"],
    description: str,
    active_ticket: str,
    librarian: "Librarian"
) -> "AdapterResult":
    if choice == "A":
        payload = build_tquery_payload(  # TREQ_013
            trigger_type="KNOWLEDGE_GAP",
            originating_agent="Tech Lead",
            active_branch=f"dev/{active_ticket}-*",
            problem_statement=description
        )
        result = librarian.write_tquery(payload)
        librarian.freeze_branch(active_ticket)
        return result
    elif choice == "B":
        return librarian.create_dev_ticket(
            type="BUG", status="PROPOSED",
            depends_on=[active_ticket], description=description)
    elif choice == "C":
        return librarian.create_dev_ticket(
            type="DEBT", status="PROPOSED",
            version_tag_milestone="BACKLOG", description=description)
    elif choice == "D":
        payload = build_bprop_payload(  # TREQ_023
            originating_layer="Technical",
            originating_agent="Tech Lead",
            title=description[:80], proposal_summary=description)
        return librarian.write_bprop(payload)
    else:  # E
        return librarian.append_future_horizon_entry(
            layer="technical", content=description)
```

* Choice A is the only option that freezes the active branch —
  B-E allow continued work on the current ticket.

## 3. Verification & QA Criteria
* `/log` available only during Execution Mode.
* `<<DEV_LOG_SUGGESTED>>` directive recognized and offered as
  suggestion, never auto-triggers `/log`.
* Choice A creates TQUERY and freezes active ticket's branch.
* Choices B-E do not freeze the branch — work continues.
* Each choice routes to the correct pipeline per TDR_017 mapping.
