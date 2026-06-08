---
extends_schema: "/governance/templates/master_metadata.yaml"
id: GIR_XXX
type: GIR
title: "Governance Integrity Report: [Phase] — [Short Description]"
status: DRAFT
relationships:
  vertical:
    governing_bdr: BDR_018
    parent_id: null
  horizontal:
    depends_on: []
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "[ISO-8601]" }
  hitl_signatory: null
---

# Governance Integrity Report: GIR_XXX — [Phase] Audit

## 1. Audit Metadata
* **Audit Phase:** [Phase 1 | Phase 2 | etc.]
* **Audit Iteration:** [e.g., Iteration 1 of N]
* **Artifacts Audited:** [Total count]
* **Context Dossier Date:** [ISO-8601]
* **Previous GIR:** [ID of superseded GIR or null if first iteration]

---

## 2. Executive Summary
* **Total Green Items:** [N]
* **Total Yellow Items:** [N]
* **Total Red Items:** [N]
* **Phase 2 Authorization Status:** [BLOCKED | CONDITIONALLY AUTHORIZED | AUTHORIZED]

---

## 3. Green Items — Ready

### GIR_XXX-G001 — [Artifact ID or Chain Segment]
* **Plane:** [1-5]
* **Verification:** [What was verified and confirmed]

---

## 4. Yellow Items — Deferrable

### GIR_XXX-Y001 — [Artifact ID or Gap Description]
* **Plane:** [1-5]
* **What:** [Specific artifact, gap, or ambiguity]
* **Why it matters:** [Type of problem]
* **When it manifests:** [During sprint | At integration | In production | Post-launch]
* **Resolution criteria:** [What constitutes a sufficient fix]
* **Inherited from ingestion check:** [YES — [Layer] transition on [Date] | NO]

**HITL Deferral Declaration:**
* Deferral justified: [ ] YES | [ ] NO — promoting to red
* Justification: ___________________________
* Planned resolution date: ___________________________
* HITL signatory: ___________________________

---

## 5. Red Items — Blocking

### GIR_XXX-R001 — [Artifact ID or Gap Description]
* **Plane:** [1-5]
* **What:** [Specific artifact, gap, orphan, or violation]
* **Why it matters:** [Type of problem and severity]
* **When it manifests:** [During sprint | At integration | In production | Post-launch]
* **Resolution criteria:** [Exact definition of done for this item]
* **Assigned to:** [Agent or HITL responsible]

---

## 6. TQUERY Status Summary
| ID | Description | Status | Resolution Artifact |
|:---|:------------|:-------|:-------------------|
| TQ_XXX | [Description] | RESOLVED \| DEFERRED \| PENDING | [Link or null] |

---

## 7. BDR_020 Ingestion Compliance Summary
| Transition | Date | Result | Yellow Items Carried Forward |
|:-----------|:-----|:-------|:----------------------------|
| Business → Product | [Date] | PASSED \| FAILED | [N] |
| Product → Architecture | [Date] | PASSED \| FAILED | [N] |
| Architecture → Tech Lead | [Date] | PASSED \| FAILED | [N] |

---

## 8. Phase 2 Authorization Gate
* **All red items cleared:** [ ] YES | [ ] NO
* **All yellow deferrals documented:** [ ] YES | [ ] NO
* **HITL Phase 2 sign-off:** ___________________________
* **Timestamp:** ___________________________
* **Global History Ledger event:** PHASE_1_CLOSED recorded: [ ] YES | [ ] NO
