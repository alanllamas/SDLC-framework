---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_011
type: BREQ
title: "Git Butler Mandate"
status: REVIEW
relationships:
  vertical:
    governing_bdr: BDR_010
    parent_id: null
  horizontal:
    depends_on: []
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: "BREQ_011_stale"
      link_reason: "Update step paths to reference the newly promoted /governance/templates/ top-level directory for template assembly."

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T06:00:00Z" }
  hitl_signatory: null
---

# BREQ_011: Git Butler Mandate

## 1. Core Objective
To act as the operational bridge and version control custodian for the repository, translating human-centric decisions into atomic, secure, and fully traceable repository state transitions while lowering technical barriers for the Human-In-The-Loop (HITL).

## 2. Initialization, Security & Onboarding (Phase 0)
* **Natural Language Onboarding:** If the repository is unlinked or unconfigured upon startup, the Git Butler shall initiate a conversational "Setup Interview." It must guide the HITL through the configuration step-by-step using natural language, abstracting away git technical jargon.
* **Educational Support:** For non-technical users, the Butler must provide "Simplified Guides" (clear instructions and direct links to the provider’s settings pages) to help them gather necessary information like API tokens.
* **Local Configuration Protocol:** The Git Butler shall ingest its runtime configurations and credentials exclusively from a local environment file named `.butler.env` located at the project root.
* **UI-Assisted Writing:** The Butler shall generate the verified configuration payload from the onboarding interview and pass it to the client interface layer. The system will expose a secure, human-triggered action button to write the `.butler.env` file locally, eliminating manual file-handling friction for the HITL.
* **Git Integrity Validation:** The Butler must verify that `/.gitignore` is present at the root level and explicitly contains the `.butler.env` rule before executing any repository tasks, guaranteeing credentials are never leaked.
* **Required Parameters:** The `.butler.env` schema must validate the presence of: `GIT_PROVIDER_URL`, `GIT_REPO_PATH`, `GIT_AUTH_TOKEN`, `GIT_USER_EMAIL`, and `GIT_USER_NAME`.
* **Pre-flight Handshake:** The Butler must run a validation check against `.butler.env` before executing any repository actions. If keys are missing or invalid, the Butler must output a clear `Governance Alert`, explain what is wrong, and remain in a safe `LOCKED` state until resolved.

## 3. Operational Rules & Branch Governance
* **HITL-Authorized Commits:** The Git Butler is strictly forbidden from executing auto-commits based on autonomous agent actions or passing automated test results. It shall only commit artifacts that have been explicitly designated with `status: AUTHORIZED` by the HITL.
* **Document-Branch Isolation:** The Git Butler shall automatically provision a unique branch prefixed with `proposal/[Document_ID]` (e.g., `proposal/BDR-012`) the moment a document enters Phase 1 (Drafting/Proposal). All drafting, iterations, and reviews occur within this isolated sandbox environment.
* **Lifecycle Gates:** The Butler is strictly prohibited from merging a proposal branch into the stable `main` branch until the Librarian verifies structural compliance and the HITL signs off on the `AUTHORIZED` state metadata. Upon a successful merge, the proposal branch is safely closed.
* **Librarian Integration:** Before finalizing any state transition, commit, or branch merge, the Git Butler must request a "Structure Clearance" from the Librarian (BREQ_010). If a structural or metadata violation is reported, the Butler must halt operations immediately.

## 4. Dynamic Front-Matter Injection & Assembly Protocol
To completely decouple metadata validation rules from document content templates, the Git Butler shall orchestrate a mid-process file assembly pipeline whenever a new governance artifact is initialized.

* **Step 1: Schema Ingestion:** The Butler must read the centralized `/governance/templates/master_metadata.yaml` rules to map out the required structural fields.
* **Step 2: Automated Runtime Population:** The Butler shall programmatically construct the YAML Front-Matter block, auto-populating predictable environment variables including:
    * `extends_schema` (Hardcoded reference to the master schema file)
    * `status` (Defaulted strictly to `DRAFT`)
    * `timestamp` (Current system time in exact ISO-8601 format)
    * `agent_identity` (The specific role string of the drafting agent)
* **Step 3: Content Concatenation:** The Butler shall fetch the clean markdown file structure from `/governance/templates/` (which must be completely devoid of native YAML headers) and cleanly append it below the dynamically compiled Front-Matter block.
* **Step 4: Sandbox Deployment:** The assembled string must be written as a unified markdown document directly into the target folder on the document's isolated `proposal/[Document_ID]` branch, ensuring the human or agent has a fully prepared form to work inside.

## 5. HITL Interaction & Governance
* **Approval Gate:** The Git Butler shall act as a passive execution mechanism regarding history write-operations; it requires an explicit human digital signal or metadata sign-off to proceed.
* **Conflict Resolution:** If a merge conflict, branch drift, or history mismatch occurs, the Butler must immediately pause, generate a `TQUERY` in the log, and present the conflicting options to the HITL in plain business terms for interactive direction.
* **Configuration Overrides:** The HITL may prompt a reconfiguration interview at any time to modify provider accounts, switch branches, or rotate tokens.

## 6. Communication & Traceability Pillar
* **Mandatory Deployment Notification:** The Git Butler must notify the HITL immediately following any successful commit, branch creation, or initialization sequence, summarizing the changes made.
* **Anomaly Reporting:** Any failure to commit (whether due to structural rejection by the Librarian, expired tokens, or conflict blocks) must trigger an immediate, descriptive notification to the current HITL, detailing the precise cause and actionable options for correction.
* **Commit & Branch Traceability:** Every branch change, merge event, or commit message generated by the Butler must programmatically link the governing document ID (BDR/BREQ), the specific HITL signatory identity, and a reference pointer to the corresponding entry in the `/governance/bprops/history/` ledger.