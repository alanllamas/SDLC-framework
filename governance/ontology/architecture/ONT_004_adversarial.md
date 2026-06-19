---
id: ONT_004
concept: "yaml-state-machine-atomic-writes-agent-session-persistence"
layer: "architecture"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_004_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_004 — yaml-state-machine-atomic-writes-agent-session-persistence
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
Complejidad del estado crece no-linealmente y YAML se vuelve inmanejable, o LangGraph ofrece state management superior sin overhead significativo.

## Tensión Central
YAML + atomic writes es correcto para Developer Solo con sesiones bounded. Riesgo real: durabilidad en OS crash — os.replace() es atómico a nivel filesystem pero no sobrevive kernel panic. SQLite WAL tiene durabilidad más robusta.

## Evidencia a Favor
Microsoft Agent Framework usa YAML declarativo para workflows en producción. LangGraph usa Python state machine — valida el approach.

## Evidencia en Contra
Sin durabilidad garantizada en kernel panic. Temporal/Inngest/Trigger diseñados para este problema exacto.

## Alternativa Lateral
SQLite como persistence layer — mismo archivo local, mejor durabilidad, queries más expresivas. Sin infraestructura adicional.

## Veredicto: CONFIRMADA v1.0 / REQUIERE_REVISIÓN v2.0+
YAML correcto para v1.0. Para v2.0+ con sesiones largas: evaluar SQLite como persistence layer o LangGraph con checkpointing integrado.

## Fuentes Verificadas
- [spaceo.ai Agentic AI Frameworks](https://www.spaceo.ai/blog/agentic-ai-frameworks/) — May 2026
- [alexcloudstar Agent Frameworks](https://www.alexcloudstar.com/blog/ai-agent-frameworks-comparison-2026/) — Apr 2026
- [LangChain Best Frameworks 2026](https://www.langchain.com/resources/ai-agent-frameworks) — Jun 2026
