---
extends_schema: "/governance/templates/master_metadata.yaml"
id: ANNOTATION_DICT_01
type: TEMPLATE_RULES
title: "Inline Annotation Tag Dictionary (Agent 7 Parser Specification)"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_011
    parent_id: BREQ_020
  horizontal:
    associated_template: "/governance/templates/TECHNICAL_DOC_template.md"

provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-01T18:05:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Inline Annotation Tag Dictionary

This document establishes the strict mechanical parsing dictionary for **Agent 7 (The Technical Process Chronicler)**. Developers must prepend these explicit tags to their inline code annotations to allow Agent 7 to parse and update documentation automatically.

## 1. Core Parsing Tags

### A. The Traceability Tag (`@trace`)
* **Purpose:** Binds a specific block of code directly to its parent requirement or architecture asset.
* **Extraction Behavior:** Agent 7 extracts the ID and populates the `parent_id` metadata tag.
* **Syntax:** `@trace [BREQ_ID or ADR_ID]`
* **Code Example (Python):**
```python
  # @trace BREQ_011
  def initialize_jwt_session(user_id):
      # Code logic follows
```

### B. The Context Tag (`@intent`)
* **Purpose:** Explains the underlying business rationale driving a code implementation.
* **Extraction Behavior:** Agent 7 extracts the string and appends it to Section 3 (The Chronicle).
* **Syntax:** `@intent [Plain text prose explanation]`
* **Code Example (JavaScript):**
```javascript
  // @intent Enforcing a strict 15-minute token expiration to meet corporate compliance standards.
  const tokenExpiry = 900;
```

### C. The Boundary Tag (`@contract`)
* **Purpose:** Declares the physical schema payload structure of data blocks or API boundaries.
* **Extraction Behavior:** Agent 7 reads the raw data format and structures it inside Section 2 (Data Contracts).
* **Syntax:** `@contract [Structured key-value definitions]`
* **Code Example (TypeScript):**
```typescript
  // @contract { user_id: string, security_roles: string[] }
  interface SystemAuthPayload {
      id: string;
      roles: Array<string>;
  }
```

## 2. Parser Execution Rules
1. **Case Sensitivity:** All tags are completely lower-case. Mixed or upper-case tags will be skipped.
2. **Inheritance Boundary:** A file-level `@trace` block maps all nested functions to that ID unless explicitly overridden.
3. **Skipping Logic:** Standard comments lacking an official `@` prefix are ignored entirely.
