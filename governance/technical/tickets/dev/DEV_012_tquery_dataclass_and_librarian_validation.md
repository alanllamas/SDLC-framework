# DEV TICKET: DEV_012 — TQUERY Dataclass & Librarian Validation
**version_tag:** v0.3.0 — TQUERY Flow
**Parent TREQ:** TREQ_013
**Trace:** @trace TREQ_013

## 1. Development Process Boundaries
* Implement TQueryPayload and ResolutionOwner Pydantic models per TREQ_013
* Implement Librarian.write_tquery(payload):
  - Schema validation
  - Sequential TQ_ID assignment (scan pending/ + resolved/)
  - YAML serialization matching TQUERY_metadata.yaml
  - Atomic write to pending/
  - state.yaml.session.pending_tqueries update

## 2. Acceptance Criteria
* Invalid payload rejected before any file write
* TQ_ID assigned without collision across pending/ and resolved/
* Written YAML matches TQUERY_metadata.yaml schema
* state.yaml update and file write are part of same atomic operation

## 3. Pre-Defined Testing Assignment
See TEST_012

## 4. Documentation Assignment
User manual placeholder: "How TQUERYs Work"
