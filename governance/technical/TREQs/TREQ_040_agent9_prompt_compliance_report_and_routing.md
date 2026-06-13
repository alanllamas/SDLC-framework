---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_040
type: TREQ
title: "Agent 9 Prompt Module, Compliance Report & Gate Decision Routing"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_023
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

# Technical Requirement: TREQ_040 — Agent 9 Prompt Module, Compliance Report & Gate Decision Routing

## 1. Intent & Scope
Defines Agent 9's system prompt module per TREQ_011's pattern,
the four-check Compliance Report, and Proceed/Hold routing
per TDR_023.

## 2. Technical Constraints

### Prompt Module
```python
# /src/agents/compliance_gatekeeper.py

AGENT_ID = "Lifecycle Compliance Gatekeeper"
GOVERNING_BREQ = "BREQ_022"
# TODO:  check that prompts and project are independant from governance it shouldnt reference breqs in code, in oother project we wont have this governance to mantain the governin breqs

def get_system_prompt(state: "ProjectState", distillation: dict) -> str:
    return f"""
You are the {AGENT_ID} for the SDLC Governance Framework.
Your operational mandate is defined in {GOVERNING_BREQ}.

Your job: review the compliance findings for ticket
{state.session.active_ticket} before its PR is created, and
present them clearly. You do not make the Proceed/Hold decision
— that is the operator's call per BDR_005.

CURRENT STATE:
- Project: {state.project_id}
- Ticket under review: {state.session.active_ticket}

FINDINGS: [injected separately — list[Finding] from checklist below]

RECENT CONTEXT:
{distillation['session_summary']}
"""
```

### Four-Check Compliance Checklist
```python
def run_compliance_checks(
    ticket_id: str, test_runner: "TestRunnerAdapter",
    librarian: "Librarian", graph: dict
) -> list["Finding"]:
    findings = []
    ticket = librarian.read_dev_ticket(ticket_id)

    # 1. Test Results
    test_path = ticket.get("test_path", f"tests/{ticket_id}/")
    test_result = test_runner.run_tests(test_path)
    if not test_result.success or test_result.data["failed"] > 0:
        findings.append(Finding(
            artifact_id=ticket_id, plane=4, severity="RED",
            what=f"{test_result.data.get('failed', '?')} test(s) failing",
            why="TEST_TICKET acceptance not met",
            resolution_criteria="All tests pass"
        ))

    # 2. Chronicle Status
    chronicle_id = f"CHRONICLE_{ticket_id}"
    if not librarian.chronicle_exists_or_queued(chronicle_id):
        findings.append(Finding(
            artifact_id=ticket_id, plane=2, severity="YELLOW",
            what="No chronicle entry yet for this ticket",
            why="Agent 7 has not run for this ticket",
            resolution_criteria="Run Technical Process Chronicler"
        ))

    # 3. Commit Compliance
    non_conforming = librarian.find_non_conforming_commits(ticket_id)
    if non_conforming:
        findings.append(Finding(
            artifact_id=ticket_id, plane=4, severity="YELLOW",
            what=f"{len(non_conforming)} commit(s) without [{ticket_id}] prefix",
            why="Commit traceability incomplete per TREQ_032",
            resolution_criteria="Amend commit messages"
        ))

    # 4. Mini Coverage Check (new artifacts only)
    new_artifacts = librarian.artifacts_created_since_ticket_start(ticket_id)
    for finding in plane_3_horizontal_coverage_scoped(new_artifacts, graph, librarian):
        findings.append(finding)

    return findings
```

### Gate Decision Routing
```python
def render_compliance_report(findings: list["Finding"]) -> str:
    # Same What/Plane/Why/Resolution-criteria format as TREQ_030 GIR
    ...

def route_gate_decision(
    choice: Literal["A", "B"],
    findings: list["Finding"],
    ticket_id: str,
    librarian: "Librarian",
    orchestrator: "Orchestrator"
) -> "AdapterResult":
    if choice == "A":  # Proceed
        librarian.log_ledger_entry(
            event="COMPLIANCE_GATE_PROCEED",
            artifact_id=ticket_id,
            details=f"{len([f for f in findings if f.severity=='RED'])} red findings acknowledged"
        )
        return continue_complete_ticket(ticket_id, orchestrator)  # TREQ_032
    else:  # Hold
        red = [f for f in findings if f.severity == "RED"]
        for f in red:
            payload = build_tquery_payload(
                trigger_type="KNOWLEDGE_GAP",
                originating_agent="Lifecycle Compliance Gatekeeper",
                active_branch=f"dev/{ticket_id}-*",
                problem_statement=f"{f.what} — {f.why}"
            )
            librarian.write_tquery(payload)
        librarian.log_ledger_entry(
            event="COMPLIANCE_GATE_HOLD", artifact_id=ticket_id)
        return AdapterResult(success=True, data="held")
```

## 3. Verification & QA Criteria
* Agent 9 module conforms to TREQ_011 signature exactly.
* Test failures → RED finding with correct failed count.
* Missing chronicle → YELLOW, doesn't block.
* Non-conforming commits → YELLOW with correct count.
* New artifact with dangling cross_plane reference → RED via
  scoped plane 3 check.
* Choice A (Proceed) continues to PR creation regardless of
  findings, logs COMPLIANCE_GATE_PROCEED.
* Choice B (Hold) generates one TQUERY per RED finding via
  TREQ_013 pipeline, logs COMPLIANCE_GATE_HOLD.
* Agent 9 never makes the Proceed/Hold decision itself —
  always presents to operator.
