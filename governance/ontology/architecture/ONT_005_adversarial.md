---
id: ONT_005
concept: "single-active-agent-hitl-governance-sequential-pipeline"
layer: "architecture"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_005_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_005 — single-active-agent-hitl-governance-sequential-pipeline
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
Secuencialidad crea bottleneck inaceptable (>2 días planeación) vs frameworks paralelos (<4 horas).

## Tensión Central
Single active agent con HITL secuencial garantiza EU AI Act Art.14 compliance y claro ownership de decisiones. Costo real: velocidad — frameworks paralelos (CrewAI, AutoGen, LangGraph) pueden ser 3-5x más rápidos en tareas independientes.

## Evidencia a Favor
34% de enterprises citan security and governance como TOP factor. EU AI Act Art.14: context + authority + rationale por decisión — secuencial lo hace naturalmente.

## Evidencia en Contra
AutoGen, CrewAI, LangGraph: paralelismo nativo para tareas independientes es 3-5x más rápido.

## Alternativa Lateral
Paralelismo selectivo dentro de una capa: generar múltiples BDRs en paralelo con un solo batch sign-off del HITL.

## Veredicto: CONFIRMADA_CON_RIESGO
Secuencial correcto para transiciones entre capas (governance requiere ownership claro). Oportunidad: paralelismo selectivo DENTRO de una capa con batch sign-off — reduce tiempo sin comprometer governance.

## Fuentes Verificadas
- [CrewAI State of Agentic AI 2026](https://www.businesswire.com/news/home/20260211693427/en/) — Feb 2026
- [HITL Orchestration 2026](https://ijctjournal.org/wp-content/uploads/2026/04/Human-in-the-Loop-HITL-Orchestration-for-Agentic-Use-Cases.pdf) — Apr 2026
- [EU AI Act Article 14](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689) — 2024
