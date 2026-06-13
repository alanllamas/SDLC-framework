# DEV TICKET: DEV_036 — Agent 8 Prompt Module & Translation Queue
**version_tag:** v0.11.0 — Agent 8: Product Delivery Translator
**Parent TREQ:** TREQ_037
**Trace:** @trace TREQ_037

## 1. Development Process Boundaries
* Implement /src/agents/product_translator.py per TREQ_037,
  conforming to TREQ_011 module signature
* Implement on_document_authorized() hook — filters
  type=CHRONICLE + consumed_by_agent_8=false, populates
  state.yaml.session.translation_queue
* Implement operator notification and TREQ_012 transition wiring
  (mirrors TREQ_035 pattern)

## 2. Acceptance Criteria
* Module conforms to TREQ_011 signature exactly
* CHRONICLE sign-off with consumed=false adds to translation_queue
* Already-consumed CHRONICLE re-sign-off does not re-queue
* Operator confirmation triggers standard transition with role
  announcement

## 3. Pre-Defined Testing Assignment
See TEST_036

## 4. Documentation Assignment
User manual placeholder: "Activating the Product Delivery Translator"
