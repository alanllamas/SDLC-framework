---
extends_schema: "/governance/templates/master_metadata.yaml"
id: ANNOTATION_ADDENDUM_TEMPLATE
type: TEMPLATE_RULES
title: "Custom Inline Annotation Addendum Blueprint"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_011
    parent_id: BREQ_020
  horizontal:
    extends_core_dictionary: "/governance/templates/INLINE_ANNOTATION_DICTIONARY.md"

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T18:05:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Custom Inline Annotation Addendum: [Team/Client Name]

This document registers custom inline annotation tags authorized for use by this specific team/client.

## 1. Custom Tag Registry

### A. Custom Tag: `@[Insert Custom Tag Name]`
* **Business Purpose:** [Explain why the team needs this tag]
* **Target Extraction Behavior:** [Define where Agent 7 should route this information]
* **Syntax:** `@[tag_name] [Argument]`
* **Code Example:**
```python
  # @[tag_name] [Argument]
```

## 2. Addendum Registration Policy
1. **No Core Overriding:** Custom tags cannot duplicate or overwrite core tags (`@trace`, `@intent`, `@contract`).
2. **Authorization Gate:** This file must be marked as `STATUS: AUTHORIZED` and signed off by the HITL before Agent 7 recognizes the tokens.
