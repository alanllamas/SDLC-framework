---
extends_schema: "/governance/templates/master_metadata.yaml"
id: GATE_PR_{{PR_NUMBER}}
type: COMPLIANCE_GATEWAY
title: "Pre-Staging Compliance Gate: Feature Branch '{{BRANCH_NAME}}'"
status: PENDING_MERGE_TO_STAGING
target_consolidation_branch: staging
relationships:
  source_git_hash: "{{COMMIT_HASH}}"
  associated_agents: [Agent_7, Agent_8, Agent_9]
---

# Pre-Staging Compliance Gate (PR #{{PR_NUMBER}})

**ACTION REQUIRED:** This package must be verified and signed off before code can merge into the shared `staging` branch. Verify the side-by-side structural mapping and use the temporary sandbox environment below to physically validate the functionality.

---

## 1. The Sandbox & Execution Verification Plane
*To prevent Staging contamination, an isolated, ephemeral preview environment has been spun up for this branch.*

* **Isolated Test Environment URL:** `https://preview-pr{{PR_NUMBER}}.internal.dev-ecosystem.io`
* **Seeded Verification Persona:** `{{TEST_USER_EMAIL}}`
* **Temporary Access Token/Pass:** `{{VAULT_TOKEN_LINK}}`

### Human Testing Guidance Script:
1. Navigate to the Sandbox URL above and authenticate with the provided tester persona.
2. Follow the specific testing steps detailed in the Structural Alignment Grid below to verify live feature actions.

---

## 2. Structural Alignment Grid (Reality vs. Documentation)
*This matrix pairs the physical code changes captured by Agent 7 with the translated user documentation written by Agent 8 and the live visual snapshot.*

### Row 1: [Component/Feature Name]
* **The Code Reality (Captured by Agent 7):**
    * **Exact Code Location:** `{{FILE_PATH}}` ➔ lines {{START_LINE}}-{{END_LINE}} (`def {{FUNCTION_NAME}}`)
    * **Data Contract Extracted:** `{{DATA_CONTRACT_JSON}}`
    * **Developer Annotation:** `{{DEVELOPER_INTENT_TAG}}`
* **The Documentation Output (Written by Agent 8):**
    * **Exact Documentation Location:** `{{DOC_FILE_PATH}}` ➔ Section {{SECTION_NUMBER}} ("{{SECTION_TITLE}}")
    * **The Prose Delta:** 
        > "{{PROSE_DELTA_TEXT}}"
* **The Human Interaction & Visual Plane (Where to Verify It):**
    * **Target UI Sandbox Route:** `https://preview-pr{{PR_NUMBER}}.internal.dev-ecosystem.io/{{UI_ROUTE}}`
    * **Live Runtime Interface Snapshot:**
        
        ![Automated Sandbox Component Highlight Snapshot]({{FREE_IMAGE_HOSTING_URL}})
        *(Visual Guide: Look for the red bounding box highlighting the changed element in the view above).*
        
* **Human Verification Gate:**
    * `[ ] Functional Reality & Visual Mapping Verified` | `[ ] Reject`

---

## 3. Exceptions & Unplanned Back-Ports (The Reality Ledger)
*This section highlights unpredicted changes that occurred during development, requiring your steering choice to safely update static baseline files.*

* **Detected Exception:** `{{EXCEPTIONAL_DRIFT_DESCRIPTION}}`
* **The Code Reality:** `{{DRIFT_FILE_PATH}}`
* **Agent 9 Classification:** `{{DRIFT_PLANE_CLASSIFICATION}}`
* **Proposed Documentation Mutation:** [Agent 9 pre-drafted metadata amendment for target file: `{{TARGET_FILE_PATH}}`]
* **Human Steering Decision Gate:**
    * `[ ] ACCEPT CODE & AUTO-AMEND METADATA` (Preserves static file path, updates internal provenance ledger).
    * `[ ] REJECT CODE` (Blocks merge, forces developer to refactor/delete implementation).

---

## 4. Pre-Staging Human Sign-Off Gate
*Signing below authorizes the Librarian to update the documentation metrics and permits the Git Butler to merge this code into Staging.*

* **Steering Sign-Off Seat 1: Technical & Blueprint Verification**
    * *Action:* Confirm that code contracts, paths, and ADR amendments match engineering reality.
    * *Signature:* ___________________________
* **Steering Sign-Off Seat 2: Product & Business Verification**
    * *Action:* Confirm that sandbox UI behavior and Agent 8 prose match product value and style standards.
    * *Signature:* ___________________________