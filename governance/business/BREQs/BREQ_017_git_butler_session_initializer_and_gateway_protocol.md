---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_017
type: BREQ
title: "Git Butler — Session Initializer & Gateway Protocol"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_009
    parent_id: null
  horizontal:
    depends_on: [BREQ_011, BREQ_014, BREQ_015]
  cross_plane:
    architecture_adr: [ADR_002]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BREQ_017: Git Butler — Session Initializer & Gateway Protocol

## 1. Core Objective
To formally establish the Git Butler as the mandatory
entry point for every system session, responsible for
verifying the operational environment, loading operator
context, and activating the correct phase agent before
any governance work begins. No agent may become active
without the Git Butler's explicit clearance.

## 2. Session Gateway Sequence

### Step 1 — Environment Verification
1. Verify runtime environment integrity:
   * Confirm `.gitignore` contains `.butler.env` per BREQ_010.
   * Execute credential discovery per ADR_002.
   * Confirm repository state per ADR_002 bootstrap protocol.
2. If any verification fails, Git Butler must resolve
   the failure before proceeding.

### Step 2 — Operator Profile Load
1. Detect operator identity from session context.
2. Load operator profile per BREQ_015.
3. If no operator profile exists, initiate first-time
   registration per PREQ_001 before proceeding.

### Step 3 — Session State Detection
1. Scan active proposal branches for interrupted or
   in-progress work per PREQ_005 section 2.3.
2. Compile and present session state summary per
   PREQ_001 returning operator flow.

### Step 4 — Phase Agent Activation
1. Determine correct phase agent based on active
   project's current lifecycle state.
2. Present operator with detected phase and agent —
   operator confirms or selects different entry point.
3. Upon confirmation Git Butler hands off to the
   activated phase agent and steps into its operational
   role as version control custodian per BREQ_011.

## 3. Credential Persistence Rule
* Git Butler only writes credentials to `.butler.env`
  that were not already found during discovery.
  Discovery is additive — never duplicative.
* If credentials are detected as expired or invalid,
  Git Butler must notify the operator and offer explicit
  choice of persistence location: system environment
  variables or `.butler.env`.
* Credential files follow ADR_002 section 4.4 —
  securely deleted when regenerated, never archived.

## 4. Lateral Credential Assistance
As a natural consequence of its gateway role, the Git
Butler may provide contextual guidance:
* Token generation instructions for GitHub.
* SSH key setup guidance.
* Permission validation and scope diagnosis.
* This is contextual guidance only — not a credential manager.

## 5. Gateway Hardlocks
* No phase agent may activate without Git Butler session clearance.
* Git Butler may not skip any gateway step.
* Direct agent invocation without gateway clearance must
  be redirected to the gateway sequence.

## 6. Business Compliance & QA Metrics
* **Metric 1:** 100% of sessions must pass through the
  gateway sequence. Zero direct agent activations.
* **Metric 2:** 100% of sessions must confirm clean
  environment state before handoff.
* **Metric 3:** 0% of pre-existing credentials may be
  duplicated into `.butler.env`.
* **Metric 4:** 100% of sessions must confirm operator
  profile load or complete registration before activation.
* **Metric 5:** 100% of invalid credential detections must
  trigger recovery flow with explicit operator choice of
  persistence location.
