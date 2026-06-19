---
id: ONT_001
concept: "vendor-agnostic-adapter-pattern-for-llm-agents"
layer: "architecture"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_001_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_001 — vendor-agnostic-adapter-pattern-for-llm-agents
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
MCP ya resuelve completamente nuestro caso de uso y el ABC propio crea más deuda que valor.

## Tensión Central
MCP es el estándar de facto (10K+ servidores, 97M SDK downloads/mes) pero tiene riesgos de seguridad documentados (attack surface ampliado, typosquatting, tool poisoning). ABC propio es correcto para v1.0 con 3 adapters.

## Evidencia a Favor
MCP endorsado por OpenAI, Google, Microsoft, Amazon via Linux Foundation AAIF. 10K+ servidores activos.

## Evidencia en Contra
Action tools crecieron de 27% a 65% de todos los MCP tools — attack surface dramáticamente ampliado. Supply chain risks documentados.

## Alternativa Lateral
Adoptar MCP como adapter protocol para v2.0+ en lugar de extender el ABC. Compatible con la arquitectura actual.

## Veredicto: CONFIRMADA_CON_RIESGO
ADR_001 correcto para v1.0. Future Horizon: migración a MCP en v2.0+ evaluando security posture primero.

## Fuentes Verificadas
- [MCP-38 Threat Taxonomy](https://arxiv.org/pdf/2603.18063) — 2026
- [FifthRow Enterprise AI Playbook](https://www.fifthrow.com/blog/ai-agent-orchestration-goes-enterprise) — May 2026
- [Safe Internet of Agents](https://arxiv.org/pdf/2512.00520) — 2026
