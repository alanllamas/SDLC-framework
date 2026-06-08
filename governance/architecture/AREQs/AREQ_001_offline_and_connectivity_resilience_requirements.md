---
extends_schema: "/governance/templates/master_metadata.yaml"
id: AREQ_001
type: AREQ
title: "Offline & Connectivity Resilience Requirements"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_006
    parent_id: ADR_002
  horizontal:
    depends_on: [ADR_001, ADR_002]
  cross_plane:
    architecture_adr: [ADR_001, ADR_002]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "System Architect", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Architectural Requirement: AREQ_001 — Offline & Connectivity Resilience

## 1. Intent & Scope
* **Description:** Defines required behavior when external connectivity is
  unavailable — specifically when LLM API is unreachable mid-session.
* **Business/Product Rationale:** Developer Solo cannot afford to lose planning
  work due to temporary API outage.

## 2. Technical Constraints & Bounds

### 2.1 LLM API Failure During Session
* Detection within 10 seconds of failed call.
* Immediate: preserve session state to ~/.sdlc/projects/[PRJ_ID]/context.md.
* Notify operator with explanation and options:
    * Wait & Retry (default: 30 second polling).
    * Suspend Session per PREQ_005 section 2.6.
    * Switch Provider if secondary adapter configured.

### 2.2 Degraded Performance
* Latency warning if no response within 30 seconds.
* Operator may cancel any pending LLM call at any time.

### 2.3 Offline-Safe Operations (no LLM required)
* Reading/writing governance artifacts via Librarian.
* Git operations via Git Butler.
* Reading Project Registry and Operator Profile from ~/.sdlc/.
* Displaying previously generated artifacts.

### 2.4 Operations Requiring Connectivity
* Agent reasoning, drafting, artifact generation.
* TQUERY resolution requiring agent intelligence.
* Ingestion Compliance checks requiring agent analysis.

### 2.5 State Preservation Guarantee
* Automatic snapshot to ~/.sdlc/ on every significant exchange.
* Maximum work loss bounded by last snapshot — never entire session.

## 3. HITL Intelligence & Review Loop
* Retry interval (default 30s) configurable via .butler.env.
* Operator options (Wait, Suspend, Switch) approved as minimum set for v1.0.

## 4. Verification & QA Criteria
* Metric 1: API failure detection within 10 seconds.
* Metric 2: Session state preserved within 5 seconds of failure detection.
* Metric 3: Latency warning within 30 seconds of unresponsive call.
* Metric 4: 100% of offline-safe operations complete without LLM connectivity.
