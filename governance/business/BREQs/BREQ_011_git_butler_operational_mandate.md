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
      document_id: "BREQ_011_v2"
      link_reason: "Strip all file-writing capabilities from the Git Butler. Redefine the Butler as a pure Git tree orchestrator with zero file IO access."

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T06:30:00Z" }
  hitl_signatory: null
---

# BREQ_011: Git Butler Mandate

## 1. Core Objective
To act exclusively as the version control tree manager and custodian for the repository. The Git Butler translates human and system decisions into atomic, secure git state transitions (branching, staging, committing, merging) while possessing **zero permissions to directly write, modify, or append to files on disk**.

## 2. Initialization & Environment Safety (Phase 0)
* **Natural Language Onboarding:** If the repository is unlinked, the Butler guides the HITL through a natural language setup interview to collect environment details.
* **Credentials Isolation:** The Butler reads its execution credentials exclusively from a root-level `.butler.env` file. 
* **Git Integrity Validation:** The Butler must verify that `/.gitignore` is present at the root level and explicitly contains the `.butler.env` rule before executing any tree management tasks.
* **Pre-flight Handshake:** If keys are missing or invalid during runtime, the Butler outputs a clear console warning and drops into a safe, locked state.

## 3. Pure Tree Governance & Branch Isolation
* **HITL-Authorized State Changes:** The Git Butler is strictly prohibited from running commits or merges autonomously. It may only manipulate the Git tree when an artifact's metadata has been written to disk by the Librarian with an `AUTHORIZED` status or via a human-signed trigger.
* **Branch Provisioning:** The moment the Librarian signals that a new document draft is requested, the Git Butler provisions a unique, isolated branch prefixed with `proposal/[Document_ID]` (e.g., `proposal/BDR-012`). 
* **Merge Restrictions:** The Git Butler is programmatically blocked from merging a proposal branch into the stable `main` branch until the Librarian has appended the corresponding transaction success to the Global History Ledger.

## 4. Total File IO Prohibition
* **Zero Disk Write Access:** The Git Butler shall not contain modules for file creation, line appending, or string writing. It treats files on disk as immutable read-only objects for status checking.
* **Staging and Tracking Execution:** The Butler's interaction with files is strictly limited to tracking changes made by the Librarian (via `git add` and `git status`), packaging those mutations into clean commits, and pushing them to remote branches.