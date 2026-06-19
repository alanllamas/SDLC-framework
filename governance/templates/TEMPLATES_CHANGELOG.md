# Changelog de Templates — governance/templates/

## Templates nuevos añadidos en este ciclo (2026-06-18)

| Template | Origen | Razón |
|:---------|:-------|:------|
| ONTOLOGY_metadata.yaml | BPROP_001 + TDR_026 | Capa 1 de la estructura de 3 capas de ontología |
| ONTOLOGY_digest.md | BPROP_001 + TREQ_045 | Capa 2 — única capa inyectable en runtime |
| ONTOLOGY_legal_context_scan.yaml | BREQ_025 | Capa 3 — output de Fase 0.5 |
| ONTOLOGY_source_excerpt.md | BREQ_025 sec.6 | Capa 3 — fragmentos con fair-use, no artículos completos |
| OPERATIONAL_MANDATE.md | BREQ_023 + TREQ_044 | Template genérico — HITL define ubicación final del artefacto |
| cost_summary_display.txt | TREQ_047 | Formato de salida del comando /cost |

## Templates actualizados (campos nuevos)

| Template | Campo agregado | Origen |
|:---------|:---------------|:-------|
| TQUERY_metadata.yaml | target_version | TREQ_043 — Plane 4 GIR version-aware |
| BPROP_metadata.yaml | target_version | TREQ_043 — mismo principio aplicado a BPROPs |

## Nota sobre OPERATIONAL_MANDATE.md

Este template es deliberadamente agnóstico sobre su ubicación final
en el filesystem (`governance/business/mandates/` vs otra estructura)
— esa decisión de organización quedó pendiente de definición por el
HITL en la sesión 2026-06-18. El template captura el contenido y la
relación (`governing_breq: BREQ_023`) independientemente de dónde
viva el archivo físicamente.

## Pendiente de verificar

Templates pre-existentes que podrían necesitar revisión por los
cambios de esta sesión pero no se tocaron en este ciclo:
- master_metadata.yaml (schema base) — verificar si necesita
  campos nuevos para soportar `type: MANDATE` o `type: ONTOLOGY_DIGEST`
- DEV_TICKET template — verificar si necesita el campo `type`
  (FEATURE/BUG/DEBT) de TREQ_034 formalizado como template
