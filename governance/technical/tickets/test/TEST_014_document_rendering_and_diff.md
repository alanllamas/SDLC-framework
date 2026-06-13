# TEST TICKET: TEST_014 — Document Rendering & Diff Presentation
**Parent DEV:** DEV_014 | **Parent TREQ:** TREQ_015

## 1. Test Goal
Verify document rendering correctly shows diffs for modifications
and handles long documents via chunking.

## 2. Test Environment
Mock Librarian with sample DRAFT documents of varying lengths.

## 3. Implementation Plan
* Unit: new document (no existing_path) — no diff section
* Unit: modified DRAFT — diff section present and accurate
* Unit: diff correctly shows added/removed/changed lines
* Unit: document under 2,000 tokens — single response, no chunking
* Unit: document over 2,000 tokens — chunked with [Part N of M]
* Unit: diff section never split across chunks
