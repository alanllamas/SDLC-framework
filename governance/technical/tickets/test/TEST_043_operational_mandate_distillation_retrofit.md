# TEST TICKET: TEST_043 — Operational Mandate Distillation Retrofit
**Parent DEV:** DEV_043 | **Parent TREQ:** TREQ_044

## 1. Test Goal
Verify all 7 agent modules are self-contained per AREQ_003 and
produce behaviorally-equivalent prompts without runtime access
to this repo's own `/governance/` planning documents.

## 2. Test Environment
Full copy of `/src/` with this repo's
`/governance/{business,product,architecture,technical}/`
directories removed (only `/governance/templates/` present,
simulating a distributed package).

## 3. Implementation Plan
* Unit: each of the 7 modules exposes `OPERATIONAL_MANDATE`
  as a non-empty string constant.
* Unit: `get_system_prompt()` output for each module contains
  `OPERATIONAL_MANDATE` as a substring.
* Unit: regex scan `BREQ_\d{3}|BDR_\d{3}|PREQ_\d{3}|ADR_\d{3}|TREQ_\d{3}`
  against each module's `get_system_prompt()` output (excluding
  source comments) returns zero matches.
* Integration: with this repo's own filled `/governance/` dirs
  removed from the test filesystem, instantiate each of the 7
  modules and call `get_system_prompt()` — all succeed, no
  FileNotFoundError or degraded output.
* Integration: spot-check one module (Technical Process
  Chronicler) — run it against a sample code delta with the
  distilled prompt vs. (for comparison only, not shipped) the
  full BREQ_020 text injected — verify output quality is
  equivalent, confirming the distillation captured the
  operative rules faithfully.
