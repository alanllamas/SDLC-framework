---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_012
type: BREQ
title: "The Unified TQUERY & Consultation Protocol"
status: REVIEW
relationships:
  vertical:
    governing_bdr: BDR_010
    parent_id: BDR_011
  horizontal:
    depends_on: [BREQ_011]
  cross_plane:
    architecture_adr: []
    technical_schema: ["/governance/templates/TQUERY_template.yaml"]
  history:
    supersedes_document:
      document_id: "BREQ_012_v2"
      link_reason: "Re-anchor file-writing mechanics away from originating agents, routing all TQUERY physical creation through the Librarian."

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T06:30:00Z" }
  hitl_signatory: null
---

# BREQ_012: The Unified TQUERY & Consultation Protocol

## 1. Core Objective
To establish a single, non-autonomous communication interface (`TQUERY`) used whenever an agent encounters an operational boundary, git conflict, or context gap. This protocol mandates that the Librarian acts as the exclusive compiler and physical writer of the query payload to disk.

## 2. The Three TQUERY Trigger Types
Every `TQUERY` block must explicitly categorize its block using one of the following three types:
* **`KNOWLEDGE_GAP`:** An agent lacks the business context required to proceed.
* **`SCOPE_DRIFT`:** An active artifact is drifting outside its semantic bounds or complexity budget.
* **`CONFLICT_RESOLUTION`:** The Git Butler detects an unresolvable branch collision or drift state.

## 3. The Brokerage & Assembly Pipeline (Librarian Compiler)
When an agent (e.g., PM Orchestrator) hits a block, it cannot write a `TQUERY` to the filesystem. It must execute the following communication pipeline:

1. **The In-Memory Payload:** The originating agent constructs the raw text values required by the `/governance/templates/TQUERY_template.yaml` layout completely in-memory.
2. **The Librarian Handoff:** The agent sends this raw payload data directly to the **Librarian Engine** via internal process communication.
3. **Validation & Injection:** The Librarian validates that the payload maps to the template rules, injects the `extends_schema` metadata, and programmatically compiles the final string.
4. **Physical Write:** The Librarian physically writes the `TQ_[ID].yaml` block onto the active proposal branch disk.
5. **Tree Staging:** The Git Butler detects the Librarian's file mutation, stages it (`git add`), executes a commit, and freezes further pipeline actions until human resolution is injected.