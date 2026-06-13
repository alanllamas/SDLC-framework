# TEST TICKET: TEST_008 — Git Butler Bootstrap & Gateway
**Parent DEV:** DEV_008 | **Parent TREQ:** TREQ_001, TREQ_007

## 1. Test Goal
Verify all gateway steps execute in correct order and no
agent activates without gateway clearance.

## 2. Test Environment
Mock all adapters. Temporary ~/.sdlc/ directory.
Temporary git repositories for bootstrap path testing.

## 3. Implementation Plan
* Unit: gateway steps execute in order 1→2→3→4
* Unit: missing credentials trigger interactive setup
* Unit: ~/.sdlc/ initialized on first run
* Unit: Path A detects existing repo correctly
* Unit: Path B clones remote to local path
* Unit: Path C initializes new repo with governance structure
* Integration: full gateway sequence with mock adapters produces valid session state
