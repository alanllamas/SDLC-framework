---
id: ONT_005
concept: "single-active-agent-hitl-governance-sequential-pipeline"
layer: "architecture"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "HIGH"
stale_after_months: 12
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_005_original"
    link_reason: "Versión original generada sin protocolo adversarial. Esta versión añade evidencia verificada y búsqueda en tres direcciones per TREQ_045."
---

# ONT_005 — single-active-agent-hitl-governance-sequential-pipeline
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
*(Formulada antes de buscar evidencia)*

**Esta decisión estaría equivocada si:** Esta decisión estaría equivocada si la secuencialidad crea un bottleneck inaceptable en el tiempo de planeación, o si frameworks multi-agente paralelos producen mejor output en menos tiempo.

**Señales de falsificación que buscamos:**
- Planeación completa tarda >2 días con agentes secuenciales vs <4 horas con paralelos
- Output de framework multi-agente paralelo evaluado como superior al secuencial en estudios independientes

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor

**CrewAI 2026 State of Agentic AI:**
100% de enterprises planean expansión de agentic AI. 34% citan security and governance como TOP factor. El HITL estructurado no es solo buena práctica — es lo que el mercado enterprise exige.

**EU AI Act Article 14 + NIST AI RMF:**
Oversight requirements: context (qué decisión se toma), authority (quién la toma), rationale (por qué). Nuestro cooling-off + sign-off + Ledger cumple los tres. Ningún framework paralelo que examinamos hace esto explícitamente.

**HITL Orchestration paper 2026 — ijctjournal.org:**
Tiered HITL con approval gates en decisiones críticas es el patrón documentado para governance agentica. Single active agent es la implementación más directa de este patrón.

### B — Evidencia en Contra y Riesgos

**CrewAI, AutoGen, LangGraph:**
Todos soportan multi-agente concurrente con scatter-gather. Para tareas independientes (generar 5 BDRs en paralelo), la paralelización puede ser 3-5x más rápida. La secuencialidad tiene un costo de tiempo real.

**fluence.network — Enterprise AI Platforms 2026:**
'Multi-agent systems can reduce busywork and improve outcomes, but orchestration adds operational complexity.' El tradeoff es real: velocidad vs complejidad de orquestación.

**microsoft.com — Agent Framework:**
Microsoft Agent Framework soporta paralelismo nativo con Azure AI Foundry. El mercado enterprise tiene frameworks paralelos con governance integrada — no solo frameworks secuenciales.

### C — Alternativas Laterales no Consideradas
- Paralelismo selectivo: agentes dentro de la misma capa pueden correr en paralelo (ej: generar 3 BDRs simultáneamente), mientras las transiciones entre capas siguen siendo secuenciales con HITL. Mejor de los dos mundos.
- Async HITL: en lugar de bloquear hasta que el HITL apruebe, el agente genera el siguiente artefacto en background y espera sign-off antes de usarlo. Reduce tiempo de espera sin comprometer governance.

---

## FASE 2 — Síntesis con Tensión

**Tensión central:** Single active agent garantiza HITL claro y EU AI Act compliance, pero tiene costo de velocidad. Frameworks paralelos son más rápidos pero diluyen la responsabilidad del HITL. Para governance SDLC la secuencialidad es correcta — pero hay oportunidades de paralelismo selectivo dentro de una misma capa que no comprometen governance.

**Veredicto:** CONFIRMADA_CON_RIESGO
**Razón:** Single agent secuencial es correcto para transiciones entre capas. Oportunidad: paralelismo selectivo DENTRO de una capa (ej: generar múltiples BDRs en paralelo, todos esperando un solo sign-off batch). Reduce tiempo de planeación sin comprometer el modelo de governance.

---

## Capa 3 — Fuentes Verificadas

| Fuente | URL | Fecha |
|:-------|:----|:------|
| CrewAI State of Agentic AI 2026 | https://www.businesswire.com/news/home/20260211693427/en/ | Feb 2026 |
| HITL Orchestration 2026 | https://ijctjournal.org/wp-content/uploads/2026/04/Human-in-the-Loop-HITL-Orchestration-for-Agentic-Use-Cases.pdf | Apr 2026 |
| fluence.network — Enterprise AI Platforms | https://www.fluence.network/blog/best-ai-agent-platform/ | Apr 2026 |
| Microsoft Agent Framework GA | https://www.langchain.com/resources/ai-agent-frameworks | Jun 2026 |
