---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_XXX
type: TDR
title: "Technical Decision Record: [Short Title]"
status: DRAFT
relationships:
  vertical:
    governing_bdr: null
    parent_id: "[AREQ_ID]"
  horizontal:
    depends_on: []
  cross_plane:
    architecture_adr: "[ADR_ID]"
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "[ISO-8601]" }
  hitl_signatory: null

# REDEFINITION NOTE — per TQ_009:
# TDR = Technical Decision Record (supersedes Technical Determination Record)
# Owner: Tech Lead (Agent 4)
# Chain: AREQ → TDR → TREQ → DEV_TICKET + TEST_TICKET
# Purpose: Implementation decisions within Architect boundaries.
# Requires Architect validation per BDR_020 Loop 3 compliance.
---

# Technical Decision Record: TDR_XXX — [Short Title]

## 1. Decision Context
* **Parent AREQ:** [Link to governing architectural requirement]
* **Governing ADR:** [Link to architectural decision setting boundaries]
* **Decision Scope:** [One sentence describing the implementation choice]
* **Boundaries Respected:** [Explicit confirmation of AREQ constraints honored]

## 2. The Implementation Decision (The "What")
* **Selected Approach:** [Implementation path chosen within Architect boundaries]
* **Rationale:** [Why this was selected — referencing AREQ constraints]

## 3. Evaluated Alternatives
### Option A: [Title]
* **Pros:** [Benefits within AREQ constraints]
* **Cons:** [Risks or trade-offs]

### Option B: [Title]
* **Pros:** [Benefits within AREQ constraints]
* **Cons:** [Risks or trade-offs]

## 4. AREQ Boundary Compliance
* **Constraint 1 from AREQ:** [Copied explicitly]
* **Constraint 2 from AREQ:** [Copied explicitly]
* **Compliance confirmation:** Decision does not violate parent ADR.
  If any boundary at risk → Loop 3 Architectural Compliance Challenge per BREQ_006.

## 5. Architect Validation
* **Validation required:** [ ] YES — touches AREQ boundaries | [ ] NO — fully within open space
* **Architect sign-off:** [Identity or N/A]
* **Date:** [YYYY-MM-DD or N/A]
* **Loop 3 triggered:** [ ] YES — see CR_XXX | [ ] NO

## 6. Downstream Impact
* **TREQs to generate:** [List of Technical Requirements from this decision]
* **DEV_TICKETs impacted:** [Which task tickets governed by this decision]

## 7. HITL Sign-off
* **Tech Lead Human:** [Identity authorizing this decision]
* **Date:** [YYYY-MM-DD]
