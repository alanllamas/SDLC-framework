---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_020
type: BREQ
title: "Technical Process Chronicler Mandate (Agent 7)"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_011
    parent_id: BDR_011
  horizontal:
    depends_on: [BREQ_010, BREQ_011]
  cross_plane:
    architecture_adr: []
    technical_schema: 
      - "/governance/templates/INLINE_ANNOTATION_DICTIONARY.md"
      - "/governance/templates/INLINE_ANNOTATION_ADDENDUM_template.md"
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T18:05:00Z" }
  hitl_signatory: "User via Console Approval"
---

# BREQ_020: Technical Process Chronicler Mandate (Agent 7)

## 1. Core Objective & Business Niche
The primary business purpose of the **Technical Process Chronicler (Agent 7)** is to completely eliminate the administrative overhead of maintaining internal technical documentation for development teams (whether human, agent, or hybrid tuples). 

By operating as an invisible, passive observation layer, Agent 7 ensures that the codebase's internal architecture, structural modifications, and operational steps are continuously documented in real time, preventing knowledge loss without slowing down the development cycle.

## 2. Strict Boundary Lines & Constraints
* **Strict Read-Only Enforcement (No Code Mutation):** Agent 7 is strictly forbidden from writing, refactoring, or modifying a single line of application source code. It possesses zero execution or edit permissions within the code directory.
* **No Direct File System Access:** Agent 7 cannot independently create or modify markdown documentation files on disk. It must format its gathered insights into structured, in-memory payloads and hand them off directly to the **Librarian** for schema validation and file generation.

### 2.1 The Self-Reference Loop-Gate (Anti-Drift Quarantine)
To prevent the agent from encountering a self-referential loop—where it attempts to document its own markdown files and creates infinite file drift—the agent's parsing engine is subject to a hard systemic quarantine:
* **Target Isolation:** Agent 7 is strictly forbidden from scanning, reading, or processing any file modification occurring inside the `/governance/` plane. 
* **Pure Execution Focus:** The agent's input lens is fixed *exclusively* on the functional source code directories of the application. It treats the governance and documentation folders as completely invisible.

### 2.2 The Pre-Merge Enforcement Gate (Repository Guardrail)
To ensure absolute compliance with documentation rules without relying on human memory, Agent 7 operates as a mandatory pre-merge quality gate controlled by the Git Butler and Librarian:
* **Functional Tracking Enforcement:** If a repository push contains modifications to functional code blocks, Agent 7 must verify the presence of a valid `@trace` token within the delta. If functional changes are completely undocumented, the branch compliance status is set to `FAILED`.
* **The Malformed Token Blockade:** If a developer utilizes the `@` prefix but enters a string that fails to match the Core Dictionary or an authorized Addendum file (e.g., typos like `@trase`), Agent 7 will flag this as a *Malformed Metadata Exception*. 
* **The Branch Lock:** In both failure scenarios, the Librarian will refuse to issue a security clearance, and the Git Butler will programmatically label the active Pull Request as `BLOCKED_BY_GOVERNANCE`. Code cannot be merged until formatting compliance is achieved.
* **Standard Comment Exemption:** Code commentary lacking an official `@` structural prefix shall be ignored entirely, leaving standard inner-loop development notes unaffected.

## 3. Operational Input Triggers (De-coupled Server-Side Automation)
Agent 7 operates entirely independently of the developer's local runtime environment. It requires no local installation or manual activation by developers. It triggers exclusively via server-side repository events managed by the Git Butler:
* **The Post-Push Mirror Trigger:** Activated automatically whenever a developer pushes a commit or updates an application feature branch. Agent 7 intercepts the raw file delta (the code diff) from the Git Butler, filtering out any non-code paths.
* **The PR Lifecycle Event:** Triggers when a Pull Request is opened, updated, or targeted for code review, allowing Agent 7 to build a complete delta of technical changes before human compliance verification.

## 4. Business Output Requirements
The payloads compiled by Agent 7 must be organized into a dedicated, clean directory structure separate from the execution source code:
* **Target Output Directory:** All generated technical files must be nested within `/governance/technical/`.
* **The Component Mapping Schema:** Every documentation file generated must explicitly bind a codebase component back to its ancestral tracking ID using the strict parsing specifications mapped out in the token registries.

### 4.1 The Dual-Registry Ingestion Engine (Multi-Client Extensibility)
At execution runtime, Agent 7's token compilation matrix is determined by a dual-loading protocol:
1. **The Immutable Core:** It always loads the standard global schemas defined in `/governance/templates/INLINE_ANNOTATION_DICTIONARY.md`.
2. **The Client Addendum:** It scans the repository for an authorized, localized addendum file built from the `/governance/templates/INLINE_ANNOTATION_ADDENDUM_template.md` blueprint. 
3. **The Composite Schema:** If a local team/client addendum is found and marked as authorized, its tokens are merged into the runtime parser dictionary. Custom tokens created on the fly outside of this file will be rejected as malformed.

## 5. Knowledge Gaps & Escalation (Mechanical TQUERY Integration)
Agent 7 shall not possess complex cognitive auditing logic. Instead, it relies on strict structural matching. A "Knowledge Gap" is defined strictly as a binary matching failure:
* **The Missing Ancestor Trigger:** If a code delta occurs in a functional directory, but Agent 7 cannot find a corresponding `@trace` tag linking it back to an authorized specification or ADR, a gap is declared.
* **The Ghost File Trigger:** If an entirely new module or architectural structure appears that was never declared in Phase 1 plans, a gap is declared.

### Escalation Protocol:
Upon triggering a gap, Agent 7 is forbidden from utilizing LLM inference to "guess" or "hallucinate" the developer's intent. It must:
1. Halt the documentation pipeline for that specific isolated module.
2. Formulate a standardized, in-memory TQUERY packet containing the unmapped code snippet.
3. Route the TQUERY directly to the Librarian, which will project it to the human supervisor for resolution.