---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_010
type: TREQ
title: "Feedback Triage Protocol"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_005
  cross_plane:
    architecture_adr: ADR_002
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_010 — Feedback Triage Protocol

## 1. Intent & Scope
Defines how HITL triages incoming GitHub Issues and routes
them to the correct governance artifact.

## 2. Technical Constraints

### Triage Routing Rules
| Issue Type | Governance Action | Owner |
|:-----------|:-----------------|:------|
| Bug (confirmed) | Create DEV_TICKET with bug type label | Tech Lead |
| Bug (needs investigation) | Create DEV_TICKET investigation spike | Tech Lead |
| Planning Gap | Create TQUERY routed to correct layer | Tech Lead → escalates |
| UX Feedback | Capture in 01_product_baseline.md Future Horizon | PM Orchestrator |
| Feature Proposal | Create BPROP and route via BREQ_009 | Business Catalyst |
| Vulnerability | Security Advisory — private resolution | Tech Lead + HITL |

### Triage SLA for MVP Testing Phase
* New issues triaged within 48 hours of submission.
* Issues labeled with triage result: confirmed, duplicate, wont-fix, needs-info.
* Issues tied to framework version via label version:vX.Y.Z.

### GitHub Labels Required
```
bug
planning-gap
ux
proposal
needs-triage
confirmed
duplicate
wont-fix
needs-info
version:v0.1.0 through version:v1.0.0
```

## 3. Verification & QA Criteria
* All labels created in GitHub before MVP testing begins.
* Triage routing documented and accessible to HITL.
* No issue older than 48 hours without triage label during testing.
