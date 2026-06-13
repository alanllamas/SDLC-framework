# DEV TICKET: DEV_015 — Sign-off Capture & Status Transition
**version_tag:** v0.4.0 — Document Generation & Sign-off
**Parent TREQ:** TREQ_016
**Trace:** @trace TREQ_016

## 1. Development Process Boundaries
* Implement sign_off_document() per TREQ_016 as single
  Librarian transaction
* Implement supersession validation — referenced document
  must exist and be AUTHORIZED
* Implement archive_asset() call for superseded documents
  to rollbacks/
* Implement Ledger entry generation (DOCUMENT_AUTHORIZED event)
* Wire to Orchestrator sign-off confirmation flow

## 2. Acceptance Criteria
* Non-DRAFT sign-off attempt rejected with clear error
* Invalid supersession reference rejected before any write
* Successful sign-off updates front-matter, writes file,
  generates Ledger entry — all or nothing
* Superseded document moved to rollbacks via archive_asset
  (never secure_delete)

## 3. Pre-Defined Testing Assignment
See TEST_015

## 4. Documentation Assignment
User manual placeholder: "Signing Off Documents"
