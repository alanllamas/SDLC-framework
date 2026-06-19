---
id: ONT_004
concept: "yaml-state-machine-atomic-writes-agent-session-persistence"
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
    document_id: "ONT_004_original"
    link_reason: "Versión original generada sin protocolo adversarial. Esta versión añade evidencia verificada y búsqueda en tres direcciones per TREQ_045."
---

# ONT_004 — yaml-state-machine-atomic-writes-agent-session-persistence
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
*(Formulada antes de buscar evidencia)*

**Esta decisión estaría equivocada si:** Esta decisión estaría equivocada si la complejidad del estado crece de forma no-lineal con el número de proyectos y el YAML se vuelve inmanejable, o si frameworks con state management ya resuelven esto mejor.

**Señales de falsificación que buscamos:**
- state.yaml excede 50KB en proyectos medianos y se vuelve difícil de debuggear
- LangGraph o Temporal ofrecen state management que hace el YAML propio redundante

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor

**spaceo.ai — Agentic AI Frameworks 2026:**
Microsoft Agent Framework: 'Declarative YAML and JSON agent definitions enable version-controlled workflows that can be reviewed, tested, and deployed through standard CI/CD pipelines.' YAML para state es el patrón enterprise establecido.

**LangChain — Best AI Agent Frameworks 2026:**
LangGraph es 'the framework people pick when they have outgrown chains and want to model their agent as an explicit state machine.' Valida el state machine approach — y LangGraph usa Python objects, no YAML. Ambos son válidos.

**taskade.com — AI Agent Platforms 2026:**
Gartner: 40% enterprise apps con task-specific AI agents para 2026, desde <5% en 2025. State persistence es prerequisito para cualquier agente de larga duración.

### B — Evidencia en Contra y Riesgos

**alexcloudstar.com — Agent Frameworks 2026:**
Sobre OpenAI Agents SDK: 'If your agent needs to pause for hours, survive a process restart, and resume cleanly, you are pairing it with something else (Inngest, Trigger, Vercel Workflow).' YAML en filesystem tiene el mismo problema — sin durability garantizada en crashes del OS.

**gurusup.com — Multi-Agent Frameworks 2026:**
'Multi-agent systems need coordination primitives: state checkpointing, handoff protocols, failure recovery. Building these primitives from scratch means reinventing message passing.' Nuestro YAML + atomic writes hace exactamente esto — y hay frameworks que ya lo tienen resuelto.

**firecrawl.dev — Open Source Agent Frameworks 2026:**
LangGraph tiene 'stateful workflows' con checkpointing integrado. Si usáramos LangGraph para orquestación, state management vendría incluido sin necesidad de YAML propio.

### C — Alternativas Laterales no Consideradas
- LangGraph como orquestador con state management propio — eliminaría la necesidad del state.yaml pero añadiría una dependencia de framework significativa.
- SQLite como persistence layer en lugar de YAML — más robusto para queries complejas, sigue siendo un archivo local sin infraestructura adicional.

---

## FASE 2 — Síntesis con Tensión

**Tensión central:** YAML + atomic writes es válido y funciona. El riesgo real no es el approach sino la durability en crashes de OS — archivos en filesystem no tienen las garantías de un WAL de base de datos. Para v1.0 Developer Solo es suficiente. Para v2.0+ con sesiones largas, SQLite o un framework con checkpointing propio sería más robusto.

**Veredicto:** CONFIRMADA para v1.0, REQUIERE_REVISIÓN para v2.0+
**Razón:** YAML + atomic writes es correcto para Developer Solo con sesiones bounded. Para v2.0+ con sesiones largas (horas), evaluar SQLite como persistence layer o LangGraph con checkpointing. El riesgo de durability en OS crash es real y debe ser documentado como limitación conocida.

---

## Capa 3 — Fuentes Verificadas

| Fuente | URL | Fecha |
|:-------|:----|:------|
| spaceo.ai — Agentic AI Frameworks | https://www.spaceo.ai/blog/agentic-ai-frameworks/ | May 2026 |
| alexcloudstar.com — Agent Frameworks Compared | https://www.alexcloudstar.com/blog/ai-agent-frameworks-comparison-2026/ | Apr 2026 |
| gurusup.com — Multi-Agent Frameworks | https://gurusup.com/blog/best-multi-agent-frameworks-2026 | May 2026 |
| LangChain — Best Frameworks 2026 | https://www.langchain.com/resources/ai-agent-frameworks | Jun 2026 |
