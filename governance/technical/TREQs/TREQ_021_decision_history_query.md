---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_021
type: TREQ
title: "Decision History Query Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.6.0 — Conflict Intelligence & History"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_011
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

# Technical Requirement: TREQ_021 — Decision History Query Implementation

## 1. Intent & Scope
Defines how "why was this decided", "what changed and when",
and "who decided this" queries are answered from existing
Global History Ledger entries per PREQ_008 section 2.3.

## 2. Technical Constraints

```python
class LedgerEntry(BaseModel):
    event: str  # e.g., "DOCUMENT_AUTHORIZED", "TQUERY_RESOLVED",
                 # "CIR_ESCALATED", "PHASE_1_CLOSED"
    artifact_id: str | None
    signatory: str | None
    timestamp: str
    supersedes: str | None = None
    details: str | None = None

def query_history(
    artifact_id: str | None,
    librarian: "Librarian"
) -> list[LedgerEntry]:
    entries = librarian.read_all_ledger_entries()
    if artifact_id:
        entries = [e for e in entries if e.artifact_id == artifact_id
                   or e.supersedes == artifact_id]
    return sorted(entries, key=lambda e: e.timestamp, reverse=True)
```

* "Why was this decided?" → filter entries for the artifact,
  find earliest entry, trace `artifact_id`'s `parent_id` chain
  (from front-matter) to surface governing BDR/BREQ.
* "What changed and when?" → `query_history(artifact_id)` —
  full reverse-chronological list including supersessions.
* "Who decided this?" → extract `signatory` and originating
  `agent_identity` (from artifact's `provenance_ledger`) for
  the relevant entry.
* Presentation: timeline format, most recent first, each entry
  one line by default with `[expand CIR_ID/TQ_ID/artifact_id]`
  option for full detail.

## 3. Verification & QA Criteria
* query_history() returns entries sorted reverse-chronologically.
* Filtering by artifact_id includes entries where artifact_id
  is either the subject or the superseded document.
* "Why was this decided" correctly traces parent_id chain to
  governing BDR/BREQ.
* Timeline presentation defaults to one line per entry with
  expand option.
