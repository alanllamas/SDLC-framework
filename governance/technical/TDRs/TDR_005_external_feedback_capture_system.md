---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_005
type: TDR
title: "External Feedback Capture System"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_001
  horizontal:
    depends_on: [TDR_001]
  cross_plane:
    architecture_adr: ADR_002
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_005 — External Feedback Capture System

## 1. Decision Context
* **Parent AREQ:** AREQ_001 — referenced as closest parent.
* **Governing ADR:** ADR_002 — GitHub as v1.0 Git provider.
* **Decision Scope:** How external testers submit feedback, bugs, and ideas
  during MVP testing phase.

## 2. The Implementation Decision
* **Selected Approach:** GitHub Issues with four structured templates in the
  framework repository. GitHub Security Advisories for vulnerability reports —
  private channel within same repo, no separate repo needed.
* **Rationale:** Zero additional infrastructure. Issues colocated with code
  and governance. Security Advisories provide private vulnerability reporting
  without leaking sensitive information publicly. Testers need only a GitHub
  account — no additional tools.

## 3. Evaluated Alternatives
### Option A — GitHub Issues + Security Advisories (Selected)
* **Pros:** Zero setup, familiar UX, integrated with repo, free,
  Security Advisories handle vulnerabilities privately.
* **Cons:** Testers need GitHub account.

### Option B — Separate public issues repo
* **Pros:** Complete code isolation.
* **Cons:** Fragments the project, extra maintenance overhead,
  Security Advisories already solve the vulnerability concern.

## 4. AREQ Boundary Compliance
* No additional infrastructure — uses GitHub natively per ADR_002.
* Security Advisories keep vulnerability details private per security mandate.

## 5. Architect Validation
* **Validation required:** NO — within ADR_002 open space.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_009, TREQ_010
* **Milestone:** v0.8.0 — must exist before MVP testing begins
