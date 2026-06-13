# TEST TICKET: TEST_009 — GitHub Issue Templates, Labels & Security Advisories
**Parent DEV:** DEV_009 | **Parent TREQ:** TREQ_009, TREQ_010

## 1. Test Goal
Verify all templates render correctly in GitHub, all labels
are available, and Security Advisories are enabled.

## 2. Test Environment
Live GitHub repository. No special setup needed.

## 3. Implementation Plan
* Manual: create new issue — verify all four templates appear
* Manual: verify each template has version field
* Manual: verify all labels from TREQ_010 exist in repo
* Manual: verify Security Advisories enabled in repo settings
* Manual: verify SECURITY.md present with reporting instructions
* Manual: verify CONTRIBUTING.md linked from README.md
* Manual: simulate full triage flow — submit test issue,
  label it, route it, verify routing table works
