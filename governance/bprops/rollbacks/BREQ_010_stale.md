---
id: BREQ_010
type: Business Requirement
governing_bdr: BDR_010
status: DRAFT
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T05:10:00Z" }
---

# BREQ_010: Librarian Mandate

## 1. Core Objective
To act as the sole custodian of the repository structure, ensuring that all artifacts are correctly positioned, formatted, and linked within the `/governance/` plane according to the governance constitution, while maintaining **Zero-Loss Traceability** through non-destructive rollbacks.

## 2. Operational Rules
* **Structural Enforcement:** The Librarian must validate every file interaction against the mandated tree specified in BDR_010 (ensuring the integrity of the root `/.gitignore` and the isolation of the `/governance/` subfolders) before any state change is finalized.
* **Automated Validation:** The Librarian runs pre-commit checks to ensure files contain correct front-matter metadata (e.g., `id`, `status`, `provenance_ledger`) as mandated by the governance rules.
* **Non-Destructive Rollback:** If a structural violation or unlinked metadata change is detected, the Librarian is strictly forbidden from overwriting files. Instead, it must:
    1. Snapshot the "failed" state and archive it into `/governance/bprops/rollbacks/`.
    2. Restore the active working file to its last known `AUTHORIZED` state.
    3. Update the `/governance/bprops/history/` ledger with a causal link detailing the violation and the successful recovery action.

## 3. HITL Intervention & Override ("Tampering" Protocol)
* **Override Capability:** The HITL is authorized to directly command the Librarian to force a state-rewind or manual structural adjustment to any historical commit point.
* **Interactive Negotiation:** The Librarian must gracefully accept and execute manual file movements or structural bypasses directly executed by the HITL.
* **Audit Trail:** All manual overrides, exceptions, and "tampering" events must be permanently logged in the `/governance/bprops/history/` ledger as an `HITL_OVERRIDE` event to ensure the chain of custody remains unbroken.

## 4. Communication & Traceability Pillar
* **Mandatory Notification:** The Librarian must issue an immediate "Governance Event Notification" to the HITL upon any structure-based intervention, failed validation, or rollback execution.
* **Evidence Logs:** Each notification must surface the nature of the structural failure, the corrective action applied, and direct links to the archived snapshot within `/governance/bprops/rollbacks/` for immediate human review.