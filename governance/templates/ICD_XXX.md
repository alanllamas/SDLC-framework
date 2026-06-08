---
extends_schema: "/governance/templates/master_metadata.yaml"
id: ICD_XXX
type: ICD
title: "Ingestion Compliance Declaration: [Layer] — [Date]"
status: DRAFT
relationships:
  vertical:
    governing_bdr: BDR_020
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
  generation_layer: { agent_identity: "[Receiving Agent]", timestamp: "[ISO-8601]" }
  hitl_signatory: null

# USAGE NOTE:
# Appended to receiving layer index document — not standalone file.
# Location: governance/[layer]/01_[layer]_baseline.md
# One declaration per layer transition per project.
---

# Ingestion Compliance Declaration
## [Receiving Layer] ← [Upstream Layer] — [Date]

## 1. Declaration Metadata
* **Ingesting Agent:** [Agent role]
* **Upstream Layer:** [Business | Product | Architecture]
* **Receiving Layer:** [Product | Architecture | Technical]
* **Declaration Date:** [ISO-8601]
* **Artifacts Ingested:** [Total count]

---

## 2. Step 1 — Completeness Check
* **Result:** PASSED | FAILED
* **Artifacts in AUTHORIZED/ACTIVE status:** [N of N]
* **Artifacts in DRAFT/REVIEW (blocking):** [List or NONE]
* **Resolution:** [TQUERY ID if blocked, or CLEAR]

---

## 3. Step 2 — Coverage Check

### 3.1 Covered Artifacts
| Artifact ID | Title | Downstream Descendant |
|:------------|:------|:----------------------|
| [ID] | [Title] | [PREQ_XXX \| ADR_XXX \| TDR_XXX] |

### 3.2 Skip Declarations (New This Layer)
| Artifact ID | Title | Skip Reason | Reference |
|:------------|:------|:------------|:----------|
| [ID] | [Title] | GOVERNANCE_POLICY \| DEFERRED \| COVERED_BY_SIBLING \| TQUERY_PENDING | [ref or null] |

### 3.3 Inherited Skips (From Upstream Layers)
| Artifact ID | Original Layer | Original Skip Reason | This Layer Action | Details |
|:------------|:--------------|:--------------------|:-----------------|:--------|
| [ID] | [Layer] | [Reason] | CONFIRMED \| CHALLENGED | [details] |

* **Coverage Check Result:** PASSED | PASSED WITH SKIPS | FAILED

---

## 4. Step 3 — Conflict Pre-Scan
* **Result:** PASSED | FAILED
* **Conflicts detected:** [List with CR reference or NONE]
* **Anti-goal violations:** [List or NONE]
* **Resolution:** [TQUERY ID or CLEAR]

---

## 5. Yellow Items
| Item | Description | Impact | When Manifests |
|:-----|:------------|:-------|:---------------|
| [ID] | [Description] | [Impact type] | [Sprint \| Integration \| Production] |

---

## 6. Overall Declaration Result
* **Completeness:** PASSED | FAILED
* **Coverage:** PASSED | PASSED WITH SKIPS | FAILED
* **Conflict Pre-Scan:** PASSED | FAILED
* **Overall Result:** CLEAR | PASSED WITH FLAGS | BLOCKED

---

## 7. HITL Authorization
* **Yellow items acknowledged:** [ ] YES | [ ] NO — N/A
* **Skip declarations reviewed:** [ ] YES | [ ] NO — N/A
* **Authorization to proceed:** [ ] GRANTED | [ ] BLOCKED
* **HITL signatory:** ___________________________
* **Timestamp:** ___________________________
