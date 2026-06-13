# DEV TICKET: DEV_006 — State Manager & Librarian Core
**version_tag:** v0.1.0 — Bootstrap & Session Gateway
**Parent TREQ:** TREQ_007, TREQ_008
**Trace:** @trace TREQ_007, TREQ_008

## 1. Development Process Boundaries
* Implement StateManager — reads/writes state.yaml using
  atomic pattern per TREQ_007
* Implement Pydantic v2 models for state.yaml schema per TREQ_008
* Implement Librarian core — schema validation before every write,
  rollback to last valid state on failure
* Librarian is the ONLY component that calls FilesystemAdapter write methods

## 2. Acceptance Criteria
* StateManager.load() returns typed state object
* StateManager.save() uses atomic write pattern
* Librarian.write() validates schema before writing
* Invalid schema rejected — never touches disk
* Librarian generates TQUERY payload on validation failure

## 3. Pre-Defined Testing Assignment
See TEST_006

## 4. Documentation Assignment
User manual placeholder: "Governance Engine Internals"
