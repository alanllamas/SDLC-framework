# BOOTSTRAPPING NOTES
> Temporary working agreement for the manual bootstrapping
> phase of the SDLC Governance Framework. This file is
> eliminated once the framework is operationally running.

**Created:** 2026-06-03
**Owner:** Alan Llamas (HITL) + Claude (Agent ensemble)

---

## Context
The framework is being planned and documented before it
exists as functional software. During this phase, certain
capabilities that will eventually be automated must be
performed manually. This document captures those temporary
workarounds so they are not confused with permanent
framework behavior.

---

## Active Workarounds

### WA_001 — ZIP Delivery Instead of Git Commits
* **What the framework will do:** Git Butler commits
  and pushes artifacts directly to the repo after
  each HITL sign-off.
* **What we do now:** Claude generates a ZIP of new
  artifacts at the end of each agent cycle. HITL
  downloads and integrates manually into the local repo.
* **Delivery rule:** One ZIP per agent cycle containing
  ALL artifacts created or modified during that cycle —
  regardless of which governance layer they belong to.
  This includes artifacts from other layers generated
  as a result of TQUERYs, cross-layer decisions, or
  upstream escalations resolved during the cycle.
  Example: if the Architect cycle triggers a new BDR
  via a TQUERY, that BDR goes in the Architect cycle's
  ZIP — not in a separate business cycle ZIP.
* **Never cumulative** unless explicitly requested by
  the HITL — each ZIP is scoped to its cycle only.
* **When this ends:** When Git Butler is operational.

### WA_002 — Manual TQUERY Logging
* **What the framework will do:** Agents write TQUERY
  files directly to `/governance/bprops/tqueries/`
  via the Librarian when a block is hit.
* **What we do now:** TQUERYs are generated in
  conversation and included in the cycle's ZIP.
  HITL saves them manually to the repo.
* **When this ends:** When Librarian is operational.

### WA_003 — Manual SYSTEM_REGISTRY Updates
* **What the framework will do:** Librarian updates
  SYSTEM_REGISTRY.md automatically after each
  authorized artifact is committed.
* **What we do now:** Claude generates an updated
  SYSTEM_REGISTRY.md at the end of each major cycle.
  HITL replaces the file manually in the repo.
* **When this ends:** When Librarian is operational.

### WA_004 — Agent Role Switching in Conversation
* **What the framework will do:** Each agent runs
  as a distinct configured instance with its own
  system prompt and context.
* **What we do now:** Claude switches between agent
  roles within the same conversation, announced
  explicitly at each transition.
* **Cooling-off rule:** Claude must explicitly announce
  role change and HITL must confirm before proceeding
  — mirrors the cooling-off gate of Config 2 per BREQ_014.
* **When this ends:** When agents are implemented
  as distinct configured instances.

### WA_005 — Layer Index Documents Updated Manually
* **What the framework will do:** Librarian appends
  to layer index documents after each authorized artifact.
* **What we do now:** Claude generates updated layer
  index documents at the end of each cycle. HITL
  replaces manually in the repo.
* **When this ends:** When Librarian is operational.

---

## Governance Rules to Follow During Bootstrapping

### RULE_001 — Nomenclature Standard
All artifact filenames must follow the framework's
established naming convention:
* Format: `[ARTIFACT_TYPE]_[NNN]_[snake_case_title].md`
* Examples: `BDR_018_phase_1_governance_integrity_audit.md`
* **Never use version suffixes** in filenames (e.g., `_v1`,
  `_v2`). Version evolution uses the supersession protocol
  per BDR_003 — each iteration gets a new sequential ID.
* GIR iterations example:
  ```
  GIR_001_phase_1_governance_integrity_report.md
  superseded_by: GIR_002_phase_1_governance_integrity_report.md
  ```

### RULE_002 — Future Horizon Placement
Future Horizon items must never appear inside governance
artifacts (BDRs, BREQs, ADRs, etc.). They belong
exclusively in the Layer Index Document of the layer
where they surfaced:
* Business ideas → `governance/business/01_business_request.md`
* Product ideas → `governance/product/01_product_baseline.md`
* Architecture ideas → `governance/architecture/01_architecture_baseline.md`
* Technical ideas → `governance/technical/01_technical_baseline.md`

### RULE_003 — Template Required for Every New Artifact Type
Every new artifact type defined in a BDR must have a
corresponding template file in `/governance/templates/`
before the first instance of that artifact is generated.
The BDR may be sealed without the template, but the
template must exist before the artifact type is used
in practice. Current artifact types requiring templates:
* ✅ BDR_XXX.md
* ✅ BREQ_XXX.md
* ✅ PDR_XXX.md
* ✅ PREQ_XXX.md
* ✅ ADR_XXX.md
* ✅ AREQ_XXX.md
* ✅ TDR_XXX.md (redefined — new template required)
* ✅ TREQ_XXX.md
* ✅ AER_XXX.md
* ✅ CR_XXX.md
* ✅ BPROP_XXX.md
* ✅ DEV_TICKET_XXX.md
* ✅ TEST_TICKET_XXX.md
* ✅ TQUERY_XXX.md
* ✅ GIR_XXX.md (new — template generated this cycle)
* ⚠️ TECH_DOC_template.md (exists but verify against Agent 7 mandate)

---

## Delivery Protocol (Current)

At the end of each agent cycle:
1. Claude delivers a ZIP with ALL artifacts created
   or modified during that cycle — across any layer.
2. HITL downloads, reviews, and integrates into repo.
3. HITL confirms integration before next cycle begins.
4. Claude does NOT regenerate previous cycles unless
   explicitly asked.

---

## File Location
This file lives in the repository root alongside
README.md and SYSTEM_REGISTRY.md.

---

## Known Gaps Being Tracked
* AREQs not generated — will emerge from Tech Lead
  task breakdown per architecture decision.
* ADRs 003, 004, 006 deferred — require implementation
  context from Tech Lead.
* ADR_005 in DRAFT — pending TQ_007 resolution by Tech Lead.
* TDR template needs to be updated — redefined from
  Technical Determination Record to Technical Decision
  Record per TQ_009.
* BDR_019 ingestion compliance declarations need to be
  added to layer index documents at each transition.

### RULE_004 — Never Break the Process Mid-Execution
Any governance process that has been initiated must be
completed before generating new artifacts or switching
to a different task. This applies to:
* Ingestion Compliance Checks (BDR_019) — must complete
  all three steps and generate the declaration before
  any new artifact generation begins.
* Governance Integrity Audits (BDR_018) — must complete
  all five planes and generate the GIR before Phase 2
  is authorized.
* TQUERY resolution cycles — must be fully resolved
  before the frozen branch resumes.
* Agent role transitions — must complete the cooling-off
  gate before the new role activates.

**Rationale:** Incomplete processes leave the governance
state in an undefined condition — artifacts may be
generated based on partial information, making the
audit trail unreliable and corrections expensive.

**If a process must be interrupted:** Document the
exact state at interruption in the layer index document
before stopping. Resume from that exact state in the
next session — never restart from scratch.

### RULE_005 — DRAFT vs AUTHORIZED Lifecycle
Documents follow a natural evolution cycle before
becoming immutable:

* **DRAFT state:** The document is a work in progress.
  It may be freely modified, replaced, or discarded
  without supersession, rollback, or audit trail entry.
  No immutability applies. A DRAFT is never archived
  to rollbacks — it either evolves to AUTHORIZED or
  is simply discarded.
* **AUTHORIZED state:** The document is immutable.
  Any change requires a new document that supersedes
  the authorized one per BDR_003 and BDR_008. The
  authorized document is never modified in place —
  it moves to rollbacks and the new document takes
  its place in the active governance plane.
* **The transition moment:** When the HITL signs off
  on a DRAFT, it becomes AUTHORIZED. From that moment
  forward, immutability applies.

**Practical implication during bootstrapping:**
If a DRAFT document needs to be updated before the
HITL signs off, simply update it in place — no
supersession, no rollback, no TQUERY required.
Only post-authorization changes trigger the full
supersession protocol.

### RULE_006 — ZIP Naming Convention by Version/Milestone
Every cycle ZIP delivered to the HITL must be named after
the version or milestone being worked on, to keep downloads
organized chronologically.

**Naming pattern:** `sdlc-[milestone_id].zip`

Examples:
* `sdlc-v0.1.0.zip` — Bootstrap & Session Gateway cycle
* `sdlc-v0.2.0.zip` — Agent Role Activation & Transitions cycle
* `sdlc-v0.8.0-mvp.zip` — MVP milestone cycle

If a cycle spans business/architecture work not yet tied to
a specific milestone (e.g., foundational ADRs before v0.1.0
breakdown), use a descriptive name instead:
* `sdlc-foundations.zip`
* `sdlc-arch-layer.zip`

This rule applies going forward — past ZIPs are not renamed.
