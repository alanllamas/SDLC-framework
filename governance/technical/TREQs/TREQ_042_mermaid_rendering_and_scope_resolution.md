---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_042
type: TREQ
title: "Mermaid Rendering & Scope Resolution"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_024
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_042 — Mermaid Rendering & Scope Resolution

## 1. Intent & Scope
Defines `generate_diagram()`, scope resolution, and Mermaid
output format per TDR_024.

## 2. Technical Constraints

```python
from typing import Literal

def generate_diagram(
    scope: str,  # "full" | "layer:X" | "artifact:ID"
    graph: dict,  # from build_dependency_graph(), TREQ_022
    librarian: "Librarian"
) -> str:
    if scope == "full":
        nodes, edges = graph, all_edges(graph)
    elif scope.startswith("layer:"):
        layer = scope.split(":")[1]
        nodes, edges = filter_by_layer(graph, layer, librarian)
    elif scope.startswith("artifact:"):
        artifact_id = scope.split(":")[1]
        impact = impact_map(artifact_id, graph)  # TREQ_022
        nodes = {artifact_id} | set(impact["direct"]) | set(impact["indirect"])
        edges = filter_edges_to_nodes(graph, nodes)
    else:
        raise ValueError(f"Unknown scope: {scope}")

    loop_edges = derive_loop_edges(librarian)  # from CIR history, TREQ_020

    lines = ["graph TD"]
    for artifact_id in nodes:
        title = librarian.get_artifact_title(artifact_id)
        lines.append(f'    {artifact_id}["{artifact_id}: {title}"]')
    for src, dst in edges:
        if (src, dst) in loop_edges:
            label = loop_edges[(src, dst)]  # e.g., "LOOP_2"
            lines.append(f'    {src} -.->|{label}| {dst}')
        else:
            lines.append(f'    {src} --> {dst}')
    return "\n".join(lines)
```

* `filter_by_layer()` includes artifacts whose `id` prefix
  matches the layer's artifact types (e.g., Technical →
  TDR/TREQ/DEV/TEST/CHRONICLE) plus one hop of cross-layer
  edges for context.
* `derive_loop_edges()` queries resolved CIRs (TREQ_020) and
  maps `related_artifacts` pairs to their `loop` value —
  rendered as dashed edges (`-.->`) per Mermaid syntax,
  solid edges (`-->`) for standard relationships.
* Output written to
  `governance/diagrams/[scope_slug]_[ISO-8601-timestamp].mmd`
  via atomic write per TREQ_007. No front-matter `status` —
  diagrams are reference artifacts, not subject to RULE_005.
* Any agent can call `generate_diagram()` — exposed as an
  Orchestrator command `/diagram [scope]` available in any phase.

## 3. Verification & QA Criteria
* `scope="full"` includes every artifact in the graph.
* `scope="layer:Technical"` includes only Technical-layer
  artifact types plus one-hop cross-layer context.
* `scope="artifact:[ID]"` matches `impact_map()`'s direct +
  indirect sets exactly.
* Resolved CIR with `loop="LOOP_2"` connecting two artifacts
  renders as a dashed edge labeled `LOOP_2`.
* Output is valid Mermaid `graph TD` syntax — renders without
  error in a standard Mermaid viewer.
* `/diagram` command available in any phase (not gated by
  phase_1_closed).
