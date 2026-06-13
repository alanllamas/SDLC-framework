---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_024
type: TDR
title: "Diagram Generation Engine"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.12.0 — Agent 9: Compliance Gatekeeper"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_011]
  cross_plane:
    architecture_adr: ADR_004
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Decision Record: TDR_024 — Diagram Generation Engine

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How any agent generates a visual diagram
  of the artifact chain (or a subset) on demand, closing the
  Future Horizon item the System Architect flagged during
  v0.6.0 planning ("full artifact-chain diagram with loops").

## 2. The Implementation Decision
* **Selected Approach:** A `generate_diagram(scope)` function
  reuses `build_dependency_graph()` (TDR_011/TREQ_022) — the
  same in-memory adjacency map already built for impact analysis
  — and renders it as **Mermaid** (`.mmd`) syntax. No new data
  structure, no new scan.

  `scope` options:
  - `"full"` — entire artifact chain, all layers.
  - `"layer:[BUSINESS|PRODUCT|ARCHITECTURE|TECHNICAL]"` — single
    layer's artifacts and their immediate cross-layer edges.
  - `"artifact:[ID]"` — one artifact plus its direct + indirect
    neighborhood (reuses `impact_map()` from TREQ_022 to bound
    the subgraph).

  Loop annotations (LOOP_1/2/3 per BREQ_006) are rendered as
  styled edges (dashed, labeled) when the graph contains CIR
  resolution history connecting two artifacts (from
  `query_cir_history()`, TREQ_020) — this is how "with loops"
  is satisfied without inventing new edge metadata: CIR
  resolutions ARE the loop edges.

  Output is a `.mmd` file written to
  `governance/diagrams/[scope_slug]_[timestamp].mmd` — any agent
  can generate one on request, no sign-off required (diagrams
  are derived/reference artifacts, not governance decisions —
  `status` field omitted from front-matter, not subject to
  RULE_005 lifecycle).

* **Rationale:** Every artifact already declares the
  relationships TDR_011's graph consumes — diagram generation
  is pure rendering over existing data, the cheapest possible
  way to close this Future Horizon item. Mermaid was chosen
  because it's plain text (git-diffable, matches the framework's
  human-readable-audit-trail principle) and renders natively in
  GitHub/most markdown viewers — zero additional tooling for the
  operator to view output.

## 3. Evaluated Alternatives
### Option A — Mermaid generation from existing dependency graph, on-demand, any agent (Selected)
* **Pros:** Zero new data structures, plain-text/git-diffable,
  renders natively in GitHub, loop edges derived from existing
  CIR history.
* **Cons:** Large `scope: full` diagrams may be visually dense —
  mitigated by layer/artifact scoping options.

### Option B — Graphviz/image-based diagrams
* **Pros:** More layout control.
* **Cons:** Binary output (not git-diffable), requires Graphviz
  installed — new dependency violating ADR_002's no-additional-
  infrastructure stance.

## 4. AREQ Boundary Compliance
* Pure computation over FilesystemAdapter-read data — no new
  adapter calls.
* Output write atomic per TREQ_007.

## 5. Architect Validation
* **Validation required:** NO — reuses TDR_011 graph and
  TREQ_020 CIR history, both authorized.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_042 (Mermaid rendering & scope resolution)
* **DEV_TICKETs impacted:** Diagram generation implementation
* **Resolves:** Architecture Future Horizon item — "full
  artifact-chain diagram with loops"
