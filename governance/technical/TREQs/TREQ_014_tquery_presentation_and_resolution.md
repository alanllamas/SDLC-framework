---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_014
type: TREQ
title: "TQUERY Presentation & Resolution Flow"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.3.0 — TQUERY Flow"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_007
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

# Technical Requirement: TREQ_014 — TQUERY Presentation & Resolution Flow

## 1. Intent & Scope
Defines how a written TQUERY is presented to the operator and
how the resolution is captured and persisted per PREQ_003.

## 2. Technical Constraints

### Presentation Format
Orchestrator renders pending TQUERYs using a fixed template —
never raw YAML:
```
🔍 TQUERY [TQ_NNN] — [trigger_type]

CONTEXT: [problem_statement — truncated to 200 words if longer,
with note "Full context in [file path]"]

OPTIONS:
A) [actionable_options.option_alpha]
B) [actionable_options.option_beta]
C) Custom — describe your own resolution

This blocks [active_branch] until resolved.
```

### Resolution Capture
* Operator selects A, B, or C (with custom text).
* Orchestrator constructs `hitl_decision` block:
```python
class HitlDecision(BaseModel):
    selected_option: Literal["alpha", "beta", "custom"]
    provided_input: str | None
    timestamp: str
    signatory_fingerprint: str
    resolution_artifact: str | None = None
```
* `Librarian.resolve_tquery(tq_id, hitl_decision)`:
    1. Reads pending TQUERY file.
    2. Appends `hitl_decision` block.
    3. Writes updated file.
    4. Calls `archive_asset()` to move file from
       `pending/TQ_NNN_*.yaml` to `resolved/TQ_NNN_*.yaml`
       per BDR_020 — non-destructive move.
    5. Removes TQ_ID from `state.yaml.session.pending_tqueries`.
    6. Generates Global History Ledger entry per BREQ_013.

### Branch Unfreeze
* Once `pending_tqueries` no longer contains the blocking ID,
  Orchestrator allows the originating agent's work to resume.
* If `resolution_artifact` is empty at resolution time (resolution
  doesn't immediately produce a document), Orchestrator notes
  this as an open follow-up in the context distillation per TREQ_005.

## 3. Verification & QA Criteria
* TQUERY presented using fixed template — no raw YAML shown to operator.
* Custom option (C) captures free-text input correctly.
* resolved/ file contains complete hitl_decision block.
* pending/ file no longer exists after resolution — moved, not deleted.
* state.yaml.session.pending_tqueries updated atomically.
* Branch resumes only after pending_tqueries no longer blocks it.
* Ledger entry generated for every resolution.
