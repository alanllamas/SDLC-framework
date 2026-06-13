---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_022
type: TREQ
title: "Impact Analysis Graph Implementation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.6.0 — Conflict Intelligence & History"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_011
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

# Technical Requirement: TREQ_022 — Impact Analysis Graph Implementation

## 1. Intent & Scope
Defines how direct/indirect/safe-zone impact maps are built
before a document modification per PREQ_008 section 2.2.

## 2. Technical Constraints

```python
def build_dependency_graph(librarian: "Librarian") -> dict[str, set[str]]:
    """
    Returns adjacency map: artifact_id -> set of artifact_ids
    that reference it via parent_id, depends_on, architecture_adr,
    technical_schema, or supersedes_document.
    """
    graph = {}
    for artifact in librarian.scan_all_governance_artifacts():
        rel = artifact["relationships"]
        refs = set()
        if rel["vertical"]["parent_id"]:
            refs.add(rel["vertical"]["parent_id"])
        refs.update(rel["horizontal"].get("depends_on", []))
        refs.update(rel["cross_plane"].get("architecture_adr", []))
        refs.update(rel["cross_plane"].get("technical_schema", []))
        superseded = rel["history"]["supersedes_document"]["document_id"]
        if superseded:
            refs.add(superseded)
        for ref in refs:
            graph.setdefault(ref, set()).add(artifact["id"])
    return graph

def impact_map(
    target_id: str,
    graph: dict[str, set[str]],
    indirect_depth: int = 2
) -> dict[str, list[str]]:
    direct = list(graph.get(target_id, set()))
    indirect = set()
    frontier = set(direct)
    for _ in range(indirect_depth - 1):
        next_frontier = set()
        for node in frontier:
            next_frontier.update(graph.get(node, set()))
        indirect.update(next_frontier - set(direct) - {target_id})
        frontier = next_frontier
    all_artifacts = set(librarian.scan_all_artifact_ids())
    safe_zone = all_artifacts - {target_id} - set(direct) - indirect
    return {
        "direct": direct,
        "indirect": list(indirect),
        "safe_zone": list(safe_zone)
    }
```

* `indirect_depth` configurable via `.butler.env`, default 2.
* Graph rebuilt per query — acceptable for v1.0 repo sizes
  per TDR_011. Caching deferred to Future Horizon if profiling
  shows need.
* Presentation per PREQ_008 section 2.2 — three labeled
  sections (Direct / Indirect / Safe Zone), direct and indirect
  listed by artifact ID + title, safe zone summarized as count
  unless expanded.

## 3. Verification & QA Criteria
* Graph correctly captures all four relationship reference types
  plus supersession.
* impact_map() direct set matches immediate graph neighbors exactly.
* indirect set excludes target and direct set — no duplicates.
* safe_zone = total artifacts minus target, direct, and indirect.
* indirect_depth configurable without code changes.
