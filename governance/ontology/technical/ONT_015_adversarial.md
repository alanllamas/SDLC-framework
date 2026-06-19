---
id: ONT_015
concept: "llm-automated-ontology-construction-versioned-evidence"
layer: "technical"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_015_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_015 — llm-automated-ontology-construction-versioned-evidence
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
Ontología LLM-generada demasiado ruidosa para informar decisiones sin revisión intensiva.

## Tensión Central
LLMs pueden generar drafts de ontología de forma útil. Pero BREQ_023 (HITL sign-off) hace que la "automatización completa" no sea posible ni deseable. El valor real es reducir el tiempo de generación del draft.

## Evidencia a Favor
LLMs construyen ontologías con calidad comparable a expertos en dominios especializados. LightRAG: 10x menos tokens que GraphRAG.

## Evidencia en Contra
Calidad depende de la calidad del texto fuente. Governance es texto técnico denso — puede confundir tipos de decisión.

## Alternativa Lateral
Semi-automático es exactamente lo que ya tenemos: LLM genera draft, HITL revisa. La "automatización" es la generación del draft.

## Veredicto: CONFIRMADA con alcance preciso
CONFIRMADA: LLMs generan drafts de ontología (Fase 2 TREQ_045). HITL aprueba. No es automatización completa.

## Fuentes Verificadas
- [LLM Ontology Construction arxiv 2602.01276](https://arxiv.org/abs/2602.01276) — 2026
- [LightRAG 2024](https://arxiv.org/abs/2410.05779) — 2024
