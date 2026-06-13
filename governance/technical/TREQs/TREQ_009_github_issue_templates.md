---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_009
type: TREQ
title: "GitHub Issue Templates for External Feedback"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.8.0 — MVP"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_005
  cross_plane:
    architecture_adr: ADR_002
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_009 — GitHub Issue Templates

## 1. Intent & Scope
Four GitHub Issue templates at .github/ISSUE_TEMPLATE/ before
MVP testing begins. GitHub Security Advisories enabled for
private vulnerability reporting.

## 2. Technical Constraints

### Template 1: bug_report.md
```markdown
---
name: Bug Report
about: Something the framework does incorrectly
labels: bug, needs-triage
---
**Framework Version:** [e.g., v0.3.0]
**Milestone:** [e.g., v0.3.0 — TQUERY Flow]

**What happened:**
[Clear description]

**Steps to reproduce:**
1.
2.
3.

**Expected behavior:**
[What should have happened]

**Session context:**
[Paste relevant output or state if available]

**Environment:**
- OS: [e.g., Ubuntu 22.04 / WSL2]
- Python version:
- Framework commit:
```

### Template 2: planning_gap.md
```markdown
---
name: Planning Gap
about: Something missing or incorrect in the framework planning
labels: planning-gap, needs-triage
---
**Framework Version:** [e.g., v0.5.0]
**Affected layer:** [Business | Product | Architecture | Technical]
**Affected artifact:** [e.g., PREQ_003, ADR_002]

**What's missing or incorrect:**
[Description]

**Impact:**
[What breaks or becomes unclear]

**Suggested resolution:**
[Optional]
```

### Template 3: ux_feedback.md
```markdown
---
name: UX Feedback
about: The flow works but feels confusing, slow, or unintuitive
labels: ux, needs-triage
---
**Framework Version:** [e.g., v0.4.0]
**Milestone:** [e.g., v0.4.0 — Document Generation]
**Affected flow:** [e.g., TQUERY resolution, document sign-off]

**What felt off:**
[Description]

**Suggested improvement:**
[Optional]

**Impact on your workflow:**
[Description]
```

### Template 4: feature_proposal.md
```markdown
---
name: Feature Proposal
about: An idea for something new the framework should do
labels: proposal, needs-triage
---
**Framework Version:** [e.g., v0.8.0]
**Target version you think this fits:** [e.g., v1.1.0]

**The idea:**
[Plain language description]

**Why it matters:**
[Problem solved or value added]

**Who benefits:**
[Which operator profile benefits most]
```

### GitHub Security Advisories
* Enable Security Advisories in repo settings.
* Used for vulnerability reports — private channel, separate
  from public issue templates.

## 3. Verification & QA Criteria
* All four templates present in .github/ISSUE_TEMPLATE/.
* Each template includes framework version field.
* Security Advisories enabled in repo settings.
* Labels declared in repo before templates go live.
