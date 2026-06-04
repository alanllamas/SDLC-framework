---
id: BDR_006
type: Business Decision Record
governing_bdr: BDR_003
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T03:45:00Z" }
  authorization_layer: { hitl_email: "", timestamp: "" }
---
# Business Decision Record: BDR_006 — Git as Governance Engine

## 1. Status & Metadata
* **BDR ID:** BDR_006
* **Date/Timestamp:** 2026-06-01
* **Type:** Core Infrastructure Policy
* **Impacted Scope Tracks:** All Agentic Governance Lifecycle Documents

## 2. Context & Problem Statement
In a multi-agent environment, documentation and code must maintain absolute alignment. If governance artifacts reside outside the code repository, the system risks divergence between the "plan" and the "implementation," resulting in loss of traceability and HITL synchronization.

## 3. The Business Decision (The "What")
Git is hereby established as the mandatory core versioning engine for all project artifacts, including all `TREQ`, `DEV_TICKET`, and `TEST_TICKET` files. All governance artifacts must reside within the same version-controlled repository as the source code. The project repository is defined as the single source of truth for both the architectural definition and the functional implementation.

## 4. Strategic Rationale & Consequences (The "Why")
* **Operational Unity:** Co-location enforces "Governance-as-Code," ensuring that every code change is explicitly linked to its corresponding governance artifact in the same commit history.
* **Traceability:** It allows for atomic validation; a feature cannot be merged if the associated `TREQ` and `TEST_TICKET` are not present and linked within the same repository state.
* **Downstream Alignment:** This eliminates the friction of managing external documentation syncs. Future agents are mandated to verify that the governance file path and the code path share the same root, ensuring the engine remains self-contained.