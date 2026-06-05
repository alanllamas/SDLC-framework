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
README.md and SYSTEM_REGISTRY.md. It is visible
to anyone opening the repo and signals that the
framework is in manual bootstrapping mode.

---

## Known Gaps Being Tracked
* AREQs not generated — will emerge from Tech Lead
  task breakdown per architecture decision.
* ADRs 003, 004, 006 deferred — require implementation
  context from Tech Lead.
* ADR_005 in DRAFT — pending TQ_007 resolution by
  Tech Lead.
* TQ_008 — Governance Integrity Audit — pending
  Business Catalyst resolution.
