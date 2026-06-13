---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_018
type: TREQ
title: "Vision Ledger, Active Constraints & Conflict Scan"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.5.0 — Technical Input & Scope Clarification"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_009
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

# Technical Requirement: TREQ_018 — Vision Ledger, Active Constraints & Conflict Scan

## 1. Intent & Scope
Defines the deterministic operations for routing a clarified
technology mention to either the Vision Ledger or Active
Constraints, including anti-goal and conflict scans per
PREQ_009 sections 2.3-2.5.

## 2. Technical Constraints

### Active Constraints Structure
New section in `01_business_request.md`, maintained by Librarian:
```markdown
## X. Active Technical Constraints
> Hard requirements — non-negotiable. Maintained by Librarian
> via the Technical Input Clarification flow.

| Constraint | Added | Source |
|:-----------|:------|:-------|
| [technology] | [ISO-8601] | [TQ_ID or session ref] |
```

### Routing Operations
```python
def route_technical_input(
    technology: str,
    choice: Literal["A", "B"],
    librarian: "Librarian"
) -> "AdapterResult":
    if choice == "A":
        # Anti-goal scan first
        anti_goals = librarian.read_anti_goals()  # from 01_business_request.md
        if matches_anti_goal(technology, anti_goals):
            return AdapterResult(success=False,
                error="ANTI_GOAL_VIOLATION", data=anti_goals)

        # Conflict scan against existing constraints
        existing = librarian.read_active_constraints()
        conflict = detect_conflict(technology, existing)
        if conflict:
            return AdapterResult(success=False,
                error="CONSTRAINT_CONFLICT", data=conflict)

        # Append to Active Constraints
        return librarian.append_active_constraint(technology)
    else:  # choice == "B"
        return librarian.append_vision_ledger_entry(technology, tag="PREFERENCE")
```

* `matches_anti_goal()` and `detect_conflict()` are simple
  substring/keyword matches against declared Anti-Goals
  (`01_business_request.md` section 3) and Active Constraints —
  no LLM call required for the match itself.
* On `ANTI_GOAL_VIOLATION`: Orchestrator presents the violated
  anti-goal and the two compliant options per PREQ_009 section
  2.4 (revise input / escalate to review anti-goal).
* On `CONSTRAINT_CONFLICT`: Orchestrator presents Conflict
  Intelligence Report per PREQ_007 (TREQ from v0.6.0 — if v0.6.0
  not yet implemented, this path generates a TQUERY instead as
  interim behavior).
* All appends use atomic write pattern per TREQ_007.

## 3. Verification & QA Criteria
* Choice A with anti-goal match → blocked, anti-goal + 2 options shown.
* Choice A with constraint conflict → CIR or TQUERY per availability.
* Choice A with no issues → appended to Active Constraints atomically.
* Choice B → appended to Vision Ledger with PREFERENCE tag atomically.
* Anti-goal and conflict scans require no LLM call.
