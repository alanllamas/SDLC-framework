---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_036
type: TREQ
title: "Code Delta Analysis & Chronicle Entry Generation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.10.0 — Agent 7: Technical Process Chronicler"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_019
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

# Technical Requirement: TREQ_036 — Code Delta Analysis & Chronicle Entry Generation

## 1. Intent & Scope
Defines how Agent 7 retrieves the code delta and the chronicle
entry's structure per TDR_019.

## 2. Technical Constraints

### Code Delta Retrieval
```python
def prepare_chronicle_context(
    ticket_id: str, git_butler: "GitButler", librarian: "Librarian"
) -> dict:
    ticket = librarian.read_dev_ticket(ticket_id)
    branch = f"dev/{ticket_id}-*"  # resolved to actual branch name
    delta_result = git_butler.fetch_code_delta(
        repo=librarian.get_repo_path(), branch=branch)
    return {
        "ticket": ticket,
        "code_delta": delta_result.data if delta_result.success else None,
        "files_touched": parse_files_touched(delta_result.data)
    }
```

### Chronicle Entry Structure
```markdown
---
extends_schema: "/governance/templates/master_metadata.yaml"
id: CHRONICLE_[DEV_TICKET_ID]
type: CHRONICLE
title: "Chronicle: [DEV_TICKET title]"
status: DRAFT
version_tag:
  target_version: "[inherited from ticket]"
  milestone: "[inherited from ticket]"
relationships:
  vertical:
    parent_id: "[DEV_TICKET_ID]"
consumed_by_agent_8: false
provenance_ledger:
  generation_layer: { agent_identity: "Technical Process Chronicler", timestamp: "[ISO-8601]" }
  hitl_signatory: null
---

# Chronicle: [DEV_TICKET title]

## Ticket Reference
[DEV_TICKET_ID] — [title]

## Files Touched
- path/to/file1.py
- path/to/file2.py

## Technical Summary
[2-4 paragraphs, Agent 7's analysis: what changed, how it works,
any notable implementation decisions visible in the diff that
weren't explicit in the ticket]
```

* If `fetch_code_delta()` fails (e.g., branch not found),
  Agent 7 surfaces the error — does not generate a chronicle
  entry from incomplete data.
* Generated entry presented via TDR_008 render-for-review
  (full content, no diff since this is a new document) for
  sign-off.
* On sign-off: `consumed_by_agent_8` remains `false` —
  set to `true` only by Agent 8 (v0.11.0) after incorporating
  into release notes.

## 3. Verification & QA Criteria
* fetch_code_delta() failure surfaces error, no chronicle
  entry created.
* Generated chronicle entry matches structure exactly,
  parent_id references correct DEV_TICKET_ID.
* version_tag inherited from the DEV_TICKET.
* Entry presented for sign-off via TDR_008 flow — full content,
  no diff (new document).
* consumed_by_agent_8 defaults to false and remains false after
  Agent 7 sign-off.
