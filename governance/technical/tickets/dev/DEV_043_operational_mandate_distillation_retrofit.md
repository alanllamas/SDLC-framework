# DEV TICKET: DEV_043 — Operational Mandate Distillation Retrofit
**version_tag:** v0.2.0 — Agent Role Activation (retrofit)
**Parent TREQ:** TREQ_044
**Trace:** @trace TREQ_044

## 1. Development Process Boundaries
* For each of the 7 modules listed in TREQ_044 section 2.2:
    * Read the governing artifact(s) from this repo's
      `/governance/` (development-time only).
    * Author an `OPERATIONAL_MANDATE` string constant —
      second-person imperative, distilled from the artifact's
      operative sections (Core Objective, numbered sequences,
      hardlocks/rules) — excluding YAML front-matter, Evaluated
      Alternatives, and QA Metrics sections.
    * Update `get_system_prompt()` to embed
      `OPERATIONAL_MANDATE` directly in its returned string.
    * Retain `GOVERNING_BREQ` as a code comment only.
* Add a regex-based static check (can be a standalone script
  or part of Agent 9's checks per AREQ_003 section 2.3) scanning
  all `get_system_prompt()` return values for bare governance-ID
  patterns outside comments.
* Add the test-harness scenario from AREQ_003 Metric 3 —
  instantiate all 7 modules with this repo's filled
  `/governance/{business,product,architecture,technical}/`
  directories absent.

## 2. Acceptance Criteria
* All 7 modules define `OPERATIONAL_MANDATE` and embed it in
  `get_system_prompt()`'s return value.
* Zero bare governance-ID matches in any returned prompt string
  (regex scan passes).
* Test harness succeeds with this repo's own filled governance
  directories removed — agent prompts generate without error.
* `GOVERNING_BREQ` comments preserved for traceability — Agent 7
  chronicle entry for this ticket references which BREQ each
  mandate was distilled from.

## 3. Pre-Defined Testing Assignment
See TEST_043

## 4. Documentation Assignment
User manual placeholder: "How Agent Behavior Is Defined" (internal
developer docs — explains the governance-doc-to-prompt
distillation pattern for future contributors)
