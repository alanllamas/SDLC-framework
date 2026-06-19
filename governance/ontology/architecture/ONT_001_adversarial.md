---
id: ONT_001
concept: "vendor-agnostic-adapter-pattern-for-llm-agents"
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
    document_id: "ONT_001_original"
    link_reason: "Versión original generada sin protocolo adversarial. Esta versión añade evidencia verificada y búsqueda en tres direcciones per TREQ_045."
---

# ONT_001 — vendor-agnostic-adapter-pattern-for-llm-agents
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
*(Formulada antes de buscar evidencia)*

**Esta decisión estaría equivocada si:** Esta decisión estaría equivocada si MCP ya resuelve completamente nuestro caso de uso y construir un ABC propio crea más deuda que valor.

**Señales de falsificación que buscamos:**
- MCP tiene adopción masiva y resuelve nuestros 3 adapters sin overhead
- El ABC propio diverge de MCP y crea incompatibilidad futura

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor

**MCP-38 Threat Taxonomy (arxiv 2603.18063):**
MCP es 'the de facto standard for connecting LLM-based agent systems to external tools'. 10,000+ servidores activos, 177,000 herramientas, 97M SDK downloads/mes en early 2026. OpenAI, Google, Microsoft, Amazon endorsaron MCP via Linux Foundation AAIF.

**FifthRow Enterprise Playbook Apr 2026 — fifthrow.com:**
MCP entrega conectividad vertical (agent-to-tool), A2A entrega horizontal (agent-to-agent). Juntos = foundation para multi-vendor orchestration. Gartner predice 40% enterprise apps con AI agents para fin de 2026.

**merge.dev — MCP alternatives 2025:**
LangGraph puede consumir MCP servers como tools — framework puede orquestar mientras MCP maneja integraciones estandarizadas. Hybrid approach válido.

### B — Evidencia en Contra y Riesgos

**MCP Security (arxiv 2604.05969):**
MCP expandió el attack surface dramáticamente. 'Action' tools (que modifican entornos externos) crecieron de 27% a 65% de todos los tools entre Nov 2024 y Feb 2026. Comprometer un servidor puede cascadear en workflows multi-agente.

**'Toward a Safe Internet of Agents' (arxiv 2512.00520):**
MCP introduce supply chain risks: typosquatting (mcp-github vs github-mcp), tool poisoning, command injection. El mismo estándar que habilita interoperabilidad estandariza el attack surface.

**Merge.dev análisis 2025:**
LangGraph: 'Security requires explicit implementation at every integration point. Complex chains can suffer performance issues.' MCP no resuelve seguridad — la delega al implementador.

### C — Alternativas Laterales no Consideradas
- Adoptar MCP como el adapter protocol para v2.0+ en lugar de extender el ABC propio. El ABC propio en v1.0 para los 3 adapters es correcto por simplicidad — pero la deuda técnica de divergir de MCP crece con cada nuevo adapter.
- A2A Protocol (Google + 50 partners) para comunicación agent-to-agent en escenarios multi-proyecto futuros. Complementario, no sustituto.

---

## FASE 2 — Síntesis con Tensión

**Tensión central:** MCP es el estándar de facto con adopción masiva, pero también concentra riesgos de seguridad significativos. Nuestro ABC propio para 3 adapters es correcto en v1.0 por simplicidad y control de seguridad. La deuda técnica de no adoptar MCP crece con cada adapter nuevo — y también lo hace la presión del ecosistema.

**Veredicto:** CONFIRMADA_CON_RIESGO
**Razón:** ADR_001 (ABC propio) es correcto para v1.0 con 3 adapters. Future Horizon: migración a MCP para v2.0+ cuando añadamos más herramientas — y evaluar MCP security posture antes de esa migración dado el attack surface documentado.

---

## Capa 3 — Fuentes Verificadas

| Fuente | URL | Fecha |
|:-------|:----|:------|
| MCP-38 Threat Taxonomy | https://arxiv.org/pdf/2603.18063 | 2026 |
| MCP Security at First Glance | https://arxiv.org/html/2506.13538v5 | Apr 2026 |
| Safe Internet of Agents | https://arxiv.org/pdf/2512.00520 | 2026 |
| FifthRow Enterprise AI Playbook | https://www.fifthrow.com/blog/ai-agent-orchestration-goes-enterprise | May 2026 |
| MCP Alternatives — merge.dev | https://www.merge.dev/blog/model-context-protocol-alternatives | Jun 2025 |
