---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_024
type: TREQ
title: "BPROP Adjudication & Routing to Governance Artifacts"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.7.0 — Innovation Proposal & Project Registry"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_012
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

# Technical Requirement: TREQ_024 — BPROP Adjudication & Routing

## 1. Intent & Scope
Defines how Business Catalyst presents a fully-enriched BPROP,
captures adjudication, and routes the outcome per PREQ_010
section 2.1.

## 2. Technical Constraints

### Presentation Template
```
💡 INNOVATION PROPOSAL [BPROP_NNN]
Originated: {originating_layer} ({originating_agent})

THE IDEA:
{proposal_summary}

WHY IT MATTERS:
{rationale}

LAYER PERSPECTIVES:
- [{entry.layer}] {entry.agent}: {entry.notes}
  (... one per enrichment_chain entry, in order collected)

ADJUDICATION OPTIONS:
A) Accept — generate/modify governing BDR or BREQ now
B) Defer — capture in Future Horizon Ledger for later
C) Reject — with rationale
```

### Adjudication Capture & Routing
```python
class Adjudication(BaseModel):
    decision: Literal["ACCEPT", "DEFER", "REJECT"]
    rationale: str
    resulting_artifact: str | None = None  # BDR/BREQ ID if ACCEPT
    timestamp: str
    signatory: str

def adjudicate_bprop(
    bprop_id: str,
    adjudication: Adjudication,
    librarian: "Librarian"
) -> "AdapterResult":
    # 1. Append adjudication to BPROP payload
    librarian.update_bprop_adjudication(bprop_id, adjudication)
    # 2. Routing per decision
    if adjudication.decision == "DEFER":
        librarian.append_future_horizon_entry(
            layer="business", content=bprop_summary(bprop_id))
    # ACCEPT: resulting_artifact generation happens via normal
    # Business Catalyst document generation flow (TDR_008) —
    # adjudicate_bprop() only records the link, doesn't create
    # the BDR/BREQ itself.
    # REJECT: no further action beyond recorded rationale.

    # 3. archive_asset() move pending/ → resolved/
    librarian.archive_asset(
        f"governance/bprops/pending/{bprop_id}_*.yaml",
        f"governance/bprops/resolved/{bprop_id}_*.yaml"
    )
    # 4. Remove from state.yaml.session.pending_bprops
    librarian.remove_pending_bprop(bprop_id)
    # 5. Ledger entry
    librarian.log_ledger_entry(
        event=f"BPROP_{adjudication.decision}",
        artifact_id=bprop_id,
        signatory=adjudication.signatory
    )
    return AdapterResult(success=True, data=bprop_id)
```

## 3. Verification & QA Criteria
* Presentation shows all enrichment_chain entries in collection order.
* ACCEPT records resulting_artifact link but does not itself
  generate the BDR/BREQ — generation uses TDR_008 sign-off flow.
* DEFER appends entry to Future Horizon Ledger (business baseline).
* REJECT records rationale, no further routing.
* All three outcomes: pending→resolved move, pending_bprops
  cleared, Ledger entry generated.
