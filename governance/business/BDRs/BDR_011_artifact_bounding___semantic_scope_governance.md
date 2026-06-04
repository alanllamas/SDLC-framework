---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_011
type: BDR
title: "Artifact Bounding & Semantic Scope Governance"
status: DRAFT
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
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T05:35:00Z" }
  hitl_signatory: null
---

# BDR_011: Artifact Bounding & Semantic Scope Governance

## 1. Objective
To eliminate the risks of monolithic document pollution and chaotic artifact explosion. This record establishes the geometric and semantic boundaries for all files generated within the `/governance/` plane, ensuring the filesystem remains compact, highly cohesive, and human-readable.

## 2. The Single Responsibility Principle for Files (SRP-F)
Every document created or modified within the governance directory must possess a single, atomic purpose. 

### 2.1 The Granularity Test
An artifact is correctly bounded if it satisfies the following condition:
* If a document contains two distinct requirements blocks that can be read, implemented, or tested by two different developers *without* either developer needing to read the other’s block, those requirements **must** be refactored into two separate files.

### 2.2 Complexity Budgets
To maintain neat structure and prevent text bloat, the Librarian shall flag any document that exceeds:
* **BDR / BREQ Files:** Maximum of 5 core operational dimensions or distinct rule sets per file.
* If a subject requires deeper elaboration, it must spin off a child document rather than expanding the parent file into a monolith.

## 3. Intent Anchoring (The Pre-Flight Blueprint)
To ensure strict adherence to the "One Branch Per Document Workflow," a branch cannot accept drafts without an established semantic boundary.

* **The Blueprint Block:** Before any drafting occurs in Phase 1, the generating agent must write a mandatory, maximum 3-sentence `Intent Anchor` in the document's front-matter or initial proposal block.
* **Semantic Verification:** During the generation cycle, the agent must continuously evaluate its output against this anchor. Content that shifts focus (e.g., a business requirement file attempting to dictate database schemas or UI styling) violates the anchor and must be rejected by the agent's internal verification layer.

## 4. Scope Drift & Mandatory Consultation Triggers
Agents are strictly forbidden from autonomously expanding a document's scope, auto-refactoring established boundaries, or making independent structural decisions when subjects bleed into adjacent topics.

### 4.1 The Scope Adjacency Protocol
When an agent detects that completing "Document A" inherently relies on defining an unmapped "Topic B," the agent must execute the following protocol:
1. **Halt Writing:** Immediately cease modification of the active artifact to prevent contextual pollution.
2. **Trigger Consultation Protocol:** Activate the engine's Consultation Protocol to explicitly flag the knowledge gap or scope boundary to the human.
3. **Present Actionable Gates:** Generate a structured `TQUERY` to the HITL presenting clear, non-autonomous choices:
    * *Option Alpha:* Incorporate Topic B into the current artifact because it is strictly cohesive and does not violate the Complexity Budget.
    * *Option Beta:* Pause the current branch, allow the HITL to authorize a new separate branch (`proposal/[New_ID]`), and draft a dedicated artifact to isolate the new scope.