---
id: ONT_011
concept: "cli-chat-interaction-model-agentic-sdlc-tools"
layer: "product"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "MEDIUM"
stale_after_months: 6
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_011_original"
    link_reason: "Revisión adversarial con evidencia verificada 2026."
---

# ONT_011 — CLI/Chat Interaction Model
## Digest Adversarial Verificado — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
Esta decisión estaría equivocada si el mercado está migrando
completamente hacia interfaces IDE/GUI y el CLI/chat se percibe
como herramienta de segunda clase.

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor
**LangChain — Best AI Agent Frameworks 2026:**
Claude Code (CLI), OpenAI Agents SDK (26,900 stars, Python), y GitHub
Spec Kit (CLI-first) son los frameworks con mayor tracción entre
developers técnicos. CLI sigue siendo el default para developers que
construyen herramientas de infraestructura.

**AWS Kiro CLI (re:Invent 2025):**
AWS lanzó Kiro como CLI tool con agent hooks — no como IDE por defecto.
La opción IDE existe pero CLI es el entry point demostrado en re:Invent.

### B — Evidencia en Contra y Riesgos
**taskade.com — AI Agent Platforms 2026:**
"Taskade Genesis leads for no-code agent orchestration." El mercado
de no-code está creciendo más rápido que el mercado de CLI para
casos de uso de orchestration. Si nuestros usuarios son developers
técnicos (PROFILE_LITE/STANDARD), CLI es correcto. Si queremos
alcanzar product managers y business analysts (para Business Catalyst),
el CLI puede ser un bloqueante.

**stackai.com — Best AI Agent Platforms 2026:**
"Enterprises standardizing full agent and workflow infrastructure.
Visual builders enable production workflows in weeks." Las plataformas
con interfaces visuales están ganando terreno en enterprise.

### C — Alternativas Laterales
- Progressive disclosure: CLI como core, interfaz web como opcional
  en la plataforma comercial (BDR_022). Misma lógica que PROFILE_LITE
  vs PROFILE_FULL — el CLI es el framework, la GUI es la plataforma.
- MCP server del Librarian: permite que cualquier IDE que soporte MCP
  consuma el framework sin una GUI propia. Kiro, Cursor, Claude Code —
  todos serían clientes del framework sin que tengamos que construir
  el IDE.

---

## FASE 2 — Síntesis con Tensión
CLI es correcto para el Developer Solo técnico. Para business analysts
y PMs que son usuarios naturales del Business Catalyst, el CLI puede
ser un bloqueante de adopción. La plataforma web (BDR_022) resuelve
esto para la capa comercial — el framework CLI es para developers,
la plataforma es para equipos mixtos.

**Veredicto:** CONFIRMADA para v1.0 (developer técnico)
La plataforma web (BDR_022) es el camino para usuarios no-técnicos.
Observación adicional: MCP server del Librarian como integration path
para IDEs existentes (Kiro, Cursor) — menor costo que construir GUI propia.

**Fuentes verificadas:**
| LangChain Agent Frameworks 2026 | https://www.langchain.com/resources/ai-agent-frameworks | Jun 2026 |
| taskade.com Agent Platforms | https://www.taskade.com/blog/ai-agent-platforms | Apr 2026 |
| stackai.com Enterprise Platforms | https://www.stackai.com/blog/the-best-ai-agent-and-workflow-builder-platforms-2026-guide | 2026 |
| AWS Kiro re:Invent 2025 | https://dev.to/kazuya_dev/aws-reinvent-2025-accelerate-multi-step-sdlc-with-kiro-dvt321-5h2d | 2025 |
