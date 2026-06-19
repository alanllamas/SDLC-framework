---
id: ONT_003
concept: "conservative-token-budget-context-distillation-llm-agents"
layer: "architecture"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_003_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_003 — conservative-token-budget-context-distillation-llm-agents
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
El ceiling de 9,500 es demasiado conservador para sesiones complejas, o la compact API hace innecesaria la destilación manual.

## Tensión Central
El ceiling de 9,500 tokens es correcto en principio (context rot documentado, MECW puede ser 99% menor al advertised). Pero Anthropic lanzó compact-2026-01-12 API que automatiza compresión con 68% reducción y 91% retención — puede hacer el ceiling fijo parcialmente redundante.

## Evidencia a Favor
MECW concept: context efectivo puede ser 99% menor al advertised. Context rot: degradación real antes del límite técnico.

## Evidencia en Contra
Anthropic compact-2026-01-12: compresión automática 68% reducción, 91% retención. Ceiling fijo puede ser sub-óptimo vs trigger dinámico.

## Alternativa Lateral
Ceiling dinámico: trigger compaction a 70% del budget disponible. Alineado con lo que hace la compact API de Anthropic.

## Veredicto: MATIZADA
Context distillation como principio: CONFIRMADA. Ceiling fijo de 9,500: reconsiderar. Evaluar compact API y ceiling dinámico (70% del budget disponible) para TDR_003.

## Fuentes Verificadas
- [Zylos Context Management](https://zylos.ai/research/2026-01-19-llm-context-management/) — Jan 2026
- [Zylos Context Compression](https://zylos.ai/research/2026-02-28-ai-agent-context-compression-strategies/) — Feb 2026
- [Atlan LLM Context Limitations](https://atlan.com/know/llm-context-window-limitations/) — Apr 2026
- [Redis Context Window Overflow](https://redis.io/blog/context-window-overflow/) — Jun 2026
