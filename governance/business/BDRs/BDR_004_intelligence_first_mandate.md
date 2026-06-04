---
id: BDR_004
type: Business Decision Record
source_agent: Agent 1 (Business Catalyst)
---
# Business Decision Record: BDR_004 — Intelligence-First Operational Mandate

## 1. Description
This record mandates that all system artifacts (BDRs, BREQs, TDRs) must be built upon verified human intelligence. Assumptions are explicitly prohibited in the planning and execution phases.

## 2. Core Principle: The Consultation Protocol
* **The Intelligence Gap:** Agents are strictly prohibited from filling "knowledge gaps" with predictive assumptions. 
* **The Mandate:** Before an agent transitions any artifact from `DRAFT` to `ACTIVE` (or proposes a `TDR`), it must explicitly perform a "Gap Analysis."
* **Human Consultation:** Any identified knowledge gap must be surfaced to the Human-in-the-Loop (HITL) via a structured **Consultation Request**. The agent must remain in a `HOLD` state for that specific item until the intelligence is provided.

## 3. Business Justification
* **Risk Mitigation:** Building on assumptions ("building on sand") leads to expensive architectural rework and product drift.
* **Operational Integrity:** This protocol ensures that the agents operate as *strategic extensions of human intent* rather than autonomous engines, maintaining perfect alignment with the business's evolving objectives.

## 4. Operational Enforcement
* **QA Integration:** Any proposal submitted by an agent lacking a clear "Intelligence Audit" (confirming which data points were verified by HITL) shall be rejected by the PM Orchestrator (Agent 2) and sent back for consultation.