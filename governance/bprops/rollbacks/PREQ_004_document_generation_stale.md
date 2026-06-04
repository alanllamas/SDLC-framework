---
extends_schema: "/governance/templates/master_metadata.yaml"
id: PREQ_004
type: PREQ
title: "Document Generation & HITL Sign-off Flow"
status: DEPRECATED
relationships:
  vertical:
    governing_bdr: BDR_005
    parent_id: PDR_001
  horizontal:
    depends_on: [PREQ_003, BREQ_010, BREQ_011]
  cross_plane:
    architecture_adr: []
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
    superseded_by: "PREQ_006"

provenance_ledger:
  generation_layer: { agent_identity: "PM Orchestrator", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# DEPRECATED — Superseded by PREQ_006
# Reason: Section 2.2 conflicted with Scenario 7 regarding
# full document presentation after change requests.
# See PREQ_006 for the authorized version.
