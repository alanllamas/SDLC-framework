---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_015
type: TDR
title: "Governance Integrity Report (GIR) Engine"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: AREQ_002
  horizontal:
    depends_on: [TDR_011, TDR_014]
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

# Technical Decision Record: TDR_015 — Governance Integrity Report (GIR) Engine

## 1. Decision Context
* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing ADR:** ADR_004 — Governance State Machine
* **Decision Scope:** How the five BDR_018 verification planes
  are implemented as deterministic checks, how findings are
  classified Green/Yellow/Red, and how the GIR document is
  generated with supersession chain, gating
  `phase_gates.phase_1_closed`.

## 2. The Implementation Decision
* **Selected Approach:** The **Five-Plane Verification Engine**
  (TREQ_029) reuses TDR_011's dependency graph
  (`build_dependency_graph()`) as its primary data structure —
  all five planes are graph traversals or Ledger queries over
  structures already built for v0.6.0:

  1. **Ascending traceability** — every artifact's `parent_id`
     chain terminates at a BDR. Orphans (chain breaks before
     reaching a BDR) → finding.
  2. **Descending traceability** — every artifact has at least
     one graph neighbor (child) OR a valid Skip Declaration
     (per BDR_020) in the relevant layer index doc. Missing
     both → finding.
  3. **Horizontal coverage** — `cross_plane` references
     (`architecture_adr`, `technical_schema`) point to
     AUTHORIZED artifacts. Dangling references → finding.
  4. **TQUERY/CIR/BPROP resolution** — `governance/bprops/*/pending/`
     must be empty. Any pending file → finding.
  5. **Anti-Goal compliance** — every entry in
     `Active Technical Constraints` (TREQ_018) and every
     AUTHORIZED artifact's stated scope re-checked against
     declared Anti-Goals (`01_business_request.md` section 3)
     using the same `matches_anti_goal()` from TREQ_018.
     Matches → finding.

  Each finding carries a severity:
  - **Red** — plane 1, 3, or 4 violations (broken chains, dangling
    refs, unresolved blockers) — always blocking.
  - **Yellow** — plane 2 violations without HITL deferral
    justification, or plane 5 matches not yet escalated.
  - **Green** — no finding for that artifact/plane.

  The **GIR Report Generator** (TREQ_030) aggregates findings into
  a `GIR_NNN.md` document using the GIR template (established
  during BDR_018 planning), with What/Plane/Why/When/Resolution-
  criteria per item. Supersession chain: `GIR_001` → `GIR_002`...
  sequential IDs per RULE_001, not version suffixes. HITL
  sign-off with zero Red items + written justification for each
  Yellow sets `state.yaml.phase_gates.phase_1_closed = true` and
  logs `PHASE_1_CLOSED` Ledger event.

* **Rationale:** All five planes reduce to traversals over the
  dependency graph (TDR_011) or scans of existing directories
  (`bprops/*/pending/`, `01_business_request.md`) — zero new data
  structures. This keeps the audit engine's correctness tied
  directly to the same relationship data every artifact already
  declares, per TDR_011's rationale.

## 3. Evaluated Alternatives
### Option A — Five planes as graph traversals/Ledger scans over existing structures (Selected)
* **Pros:** Reuses TDR_011 graph, zero new capture infrastructure,
  audit correctness tied to data artifacts already declare.
* **Cons:** Full graph rebuild per audit run — acceptable, audits
  are infrequent (once per phase) per BDR_018.

### Option B — Dedicated audit database populated incrementally
* **Pros:** Faster repeated audits.
* **Cons:** New infrastructure, sync risk, audits are infrequent
  enough that this is unjustified for v1.0.

## 4. AREQ Boundary Compliance
* All checks via FilesystemAdapter reads — no adapter calls
  beyond directory/file scans.
* GIR document write atomic per TREQ_007, sign-off via TDR_008
  sign-off flow (GIR is itself a sealed document once HITL approves).

## 5. Architect Validation
* **Validation required:** NO — implements BDR_018 five-plane
  audit already authorized at governance level, technical
  implementation only.
* **Loop 3 triggered:** NO

## 6. Downstream Impact
* **TREQs generated:** TREQ_029 (Five-Plane Verification Engine),
  TREQ_030 (GIR Report Generation & Severity Classification)
* **DEV_TICKETs impacted:** Integrity audit implementation —
  final gate before Phase 2
