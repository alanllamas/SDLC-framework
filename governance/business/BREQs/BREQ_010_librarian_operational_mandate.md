---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_010
type: BREQ
title: "Librarian Mandate"
status: REVIEW
relationships:
  vertical:
    governing_bdr: BDR_010
    parent_id: null
  horizontal:
    depends_on: []
  cross_plane:
    architecture_adr: []
    technical_schema: ["/governance/templates/master_metadata.yaml"]
  history:
    supersedes_document:
      document_id: "BREQ_010_stale"
      link_reason: "Upgrade Librarian to an active security kernel holding an absolute monopoly over File I/O operations and dynamic front-matter compilation."

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T06:35:00Z" }
  hitl_signatory: null
---

# BREQ_010: Librarian Mandate

## 1. Core Objective
To act as the exclusive custodian of the repository structure and validation rules within the Governance Engine. The Librarian serves as a secure processing kernel, ensuring all files are perfectly formatted, syntactically legal, and mapped to the established governance constitutions before any change touches physical storage.

## 2. The File I/O Monopoly (Security Kernel)
* **Zero-Bypass Access Control:** The Librarian is the only agent in the entire repository ecosystem permitted to execute physical write, rewrite, create, or append operations on the disk filesystem. 
* **Agent Isolation:** All other components (e.g., Business Catalyst, Product Orchestrator, Git Butler) are strictly blocked from file modification APIs. They must operate entirely in-memory and submit text payloads to the Librarian for processing.
* **Write Rejection State:** If a payload submitted by an agent fails any rule defined in this mandate, the Librarian shall refuse to commit the string to storage, drop the transaction, and generate an immediate `TQUERY` block.

## 3. Structural & Metadata Validation Engine
Before writing any file to disk, the Librarian must run a 3-tiered validation pass:
* **Schema Verification:** Parse the submitted front-matter against `/governance/templates/master_metadata.yaml` to ensure all fields match required data types, array formats, and key structures.
* **State Gate Enforcement:** Verify that status transitions follow strict legal pathways (e.g., a file cannot transition directly from `DRAFT` to `AUTHORIZED` without an intermediary `REVIEW` timestamp and a valid human signatory string).
* **Non-Destructive Rollbacks:** If an invalid file state is somehow introduced, the Librarian isolates the offending file, packages it into `/governance/bprops/rollbacks/` for historical audit, and restores the previous valid state from the stable tree branch.

## 4. Compilation & Assembly Pipeline Execution
The Librarian acts as the compiler for new file initialization, managing the Dynamic Front-Matter Injection Protocol:
* **Assembly Trigger:** Upon receiving an execution signal to spin up a new document, the Librarian fetches the specified clean markdown layout from `/governance/templates/`.
* **Dynamic Generation:** The Librarian reads the master schema, programmatically constructs the required YAML front-matter block, auto-injects runtime metadata values (ISO timestamps, root originating agent, schema paths), and stitches the YAML block perfectly to the top of the template text.
* **Commit Clearance:** Once assembled, the Librarian writes the file to the active proposal branch and broadcasts a `Write Cleared` token to the Git Butler to allow tree staging.

## 5. Global History Ledger Custodianship
* **Append-Only Maintenance:** The Librarian is the sole entity authorized to append rows to the transaction history ledger files in `/governance/bprops/history/`.
* **Audit Trail Calibration:** Every successfully validated document modification, supersession, or finalized human `TQUERY` resolution must be documented in the ledger by the Librarian with zero-loss traceability metrics before a branch can be merged into `main`.