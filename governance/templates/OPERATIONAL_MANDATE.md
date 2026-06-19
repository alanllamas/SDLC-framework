---
# Template: OPERATIONAL_MANDATE_[agent_name].md
# Per BREQ_023 (Epistemic Charter) + TREQ_044 (Distillation Pattern)
# Este es un artefacto de NEGOCIO (distilación de BREQ) implementado
# como constante de código — no es código en sí mismo.
id: "MANDATE_NNN"
type: "MANDATE"
title: "Operational Mandate — [Agent Name]"
status: "DRAFT"
relationships:
  vertical:
    governing_breq: "BREQ_023"  # + BREQ específico del rol si existe
    parent_id: "BREQ_023"
  horizontal:
    implemented_by: null  # TREQ que especifica el patrón de código (TREQ_044)
    realized_in: null     # path al módulo, ej "/src/agents/business_catalyst.py"
provenance_ledger:
  generation_layer: { agent_identity: "", timestamp: "" }
  hitl_signatory: null
---

# OPERATIONAL_MANDATE — [Agent Name]

[Mandato específico del rol — qué hace este agente, cómo opera,
qué riesgos típicos debe vigilar]

---

## Principios Epistémicos (BREQ_023 — todos los agentes)
[P1-P5 — Calibrated Honesty, Mandatory Counterargument,
Evidence-First Reasoning, Peer-Level Engagement, Transparent Uncertainty]

## Escala de Desacuerdo (todos los agentes)
[DUDA MENOR → RIESGO IDENTIFICADO → OBJECIÓN FUERTE → BLOQUEO ÉTICO]

## Riesgo típico de este rol
[específico al agente]

## Principios con mayor vigilancia requerida
[cuáles de P1-P5, sin implicar que los demás no aplican]
