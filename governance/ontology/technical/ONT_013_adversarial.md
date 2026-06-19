---
id: ONT_013
concept: "software-artifact-dependency-traceability-graph"
layer: "technical"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_013_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_013 — software-artifact-dependency-traceability-graph
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
El grafo en memoria es demasiado lento o frágil para 250+ artefactos.

## Tensión Central
Dependency graph para traceability validado por DHS S&T. Riesgo: arc-kit (mismo hallazgo que ONT_002) ya implementa dependency tracking entre artefactos con funcionalidad similar al Librarian.

## Evidencia a Favor
DHS S&T busca capacidades de ADG activamente. LLMs pueden inferir trace links faltantes — útil para brownfield onboarding.

## Evidencia en Contra
arc-kit implementa dependency tracking con más features. Librarian puede estar reinventando partes de arc-kit.

## Alternativa Lateral
Adoptar arc-kit como base del Librarian. Evaluar compatibilidad con jerarquía BDR→DEV_TICKET.

## Veredicto: CONFIRMADA_CON_RIESGO
CONFIRMADA. Estudiar arc-kit antes de implementar el Librarian — puede haber overlap significativo que evite trabajo redundante.

## Fuentes Verificadas
- [DHS ADG solicitation 2024](https://www.dhs.gov/science-and-technology/news/2024/08/16/st-seeks-solutions-software-artifact-dependency-graph-generation) — 2024
- [LLM Traceability arxiv 2511.02434](https://arxiv.org/abs/2511.02434) — 2025
- [arc-kit](https://github.com/tractorjuice/arc-kit) — 2026
