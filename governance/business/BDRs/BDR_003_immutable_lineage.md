---
id: BDR_003
type: Business Decision Record
source_agent: Agent 1 (Business Catalyst)
---
# Business Decision Record: BDR_003 — Immutable Lineage & Traceability

## 1. Description
This record mandates that all project artifacts—requirements, design decisions, and architectural specifications—must maintain a permanent, immutable lineage. No document is ever truly "deleted"; instead, it is either superseded or deprecated, with bi-directional linking preserved for auditability.

## 2. Core Principle: The Lineage Rule
* **Permanence:** All artifacts (`BDR`, `BREQ`, `TDR`, `ADR`) are immutable once they reach an `ACTIVE` or `COMPLETED` state.
* **Supersession Chain:** Evolution of documentation must occur exclusively through the "Supersession Chain."
    * **New Document:** Must explicitly state `supersedes: [ID_of_Previous_Doc]`.
    * **Old Document:** Must explicitly state `superseded_by: [ID_of_New_Doc]`.
* **Broken Links Prohibited:** Any documentation change that breaks a trace back to the original `BDR` or `BREQ` is considered a critical governance failure.

## 3. Business Justification
* **Auditability:** We must be able to reconstruct the "Decision Tree" of the product at any point in the future.
* **Contextual Stability:** Preventing the deletion of history ensures that future agents (or humans) understand the *evolution* of a technical requirement, not just the end state.

## 4. Operational Enforcement
* **Registry Integrity:** The `SYSTEM_REGISTRY.md` must be updated concurrently with any document creation or supersession.
* **Agent Compliance:** Agents are restricted from altering the "History of Record." They may only propose new, linked documents.