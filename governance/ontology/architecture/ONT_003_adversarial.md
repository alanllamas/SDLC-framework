---
id: ONT_003
concept: "conservative-token-budget-context-distillation-llm-agents"
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
    document_id: "ONT_003_original"
    link_reason: "Versión original generada sin protocolo adversarial. Esta versión añade evidencia verificada y búsqueda en tres direcciones per TREQ_045."
---

# ONT_003 — conservative-token-budget-context-distillation-llm-agents
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
*(Formulada antes de buscar evidencia)*

**Esta decisión estaría equivocada si:** Esta decisión estaría equivocada si el ceiling de 9,500 tokens es demasiado conservador para sesiones complejas, o si existe una técnica de compresión automática que hace innecesaria la destilación manual.

**Señales de falsificación que buscamos:**
- Sesiones reales del framework requieren más de 9,500 tokens para contexto completo sin pérdida
- Anthropic compact API o técnica similar resuelve context management sin ceiling manual

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor

**Zylos Research — Context Management Mar 2026 — zylos.ai:**
Chroma 2025: 'degradation at every increment of context growth, lost-in-the-middle effect causing 30%+ accuracy drops.' Research de Anthropic, JetBrains y SWE-agent converge: 'raw context size matters less than context quality.' 'Practical threshold for proactive management is well under 50% of nominal context capacity.'

**Atlan — LLM Context Window Limitations Apr 2026:**
Maximum Effective Context Window (MECW) concept de Paulsen 2025: effective context puede ser 99% menor al advertised. 'Attention dilution: at 100K tokens, model is managing roughly 10 billion pairwise relationships.'

**Redis — Context Window Overflow Jun 2026:**
'Context rot' como leading indicator: latency rises before explicit overflow. 'Agent workflow failures: large tool outputs overflow the context window, preventing task completion.'

### B — Evidencia en Contra y Riesgos

**Zylos Research — Context Compression Feb 2026:**
Anthropic lanzó compact-2026-01-12 beta: compaction API automática que 'trigger-and-summarize loop' a 70% de budget utilization. Reduce context 68% reteniendo 91% de información crítica. Esta API puede hacer innecesaria parte de nuestra destilación manual.

**Morph LLM — Context Window Comparison Jun 2026:**
Claude Sonnet 4.6 tiene 1M+ token context window. 'Compacting early in a 100-call agent session cuts total session cost far more than the single-request saving.' El ceiling de 9,500 puede ser muy conservador si la compact API maneja la compresión automáticamente.

**Zylos Research — Context Window Jan 2026:**
65% de enterprise AI failures en 2025 atribuidos a context drift o memory loss durante multi-step reasoning. El problema es real — pero las soluciones 2026 son más sofisticadas que un ceiling fijo.

### C — Alternativas Laterales no Consideradas
- Anthropic compact-2026-01-12 API como alternativa a destilación manual — reduce 68% tokens reteniendo 91% de info crítica. Evaluar si reemplaza o complementa TDR_003.
- Ceiling dinámico en lugar de fijo: trigger compaction cuando context supera 70% del budget disponible (alineado con lo que hace la compact API).

---

## FASE 2 — Síntesis con Tensión

**Tensión central:** El 9,500-token ceiling es correcto en principio — context rot es real y documentado. Pero Anthropic lanzó una compact API que podría hacer la destilación manual parcialmente redundante. El ceiling fijo puede ser sub-óptimo versus un trigger dinámico basado en % de budget usado.

**Veredicto:** MATIZADA
**Razón:** Context distillation como principio es correcto y validado por múltiples papers. El ceiling fijo de 9,500 tokens debe ser reconsiderado: (1) evaluar Anthropic compact-2026-01-12 API para automatizar parte del proceso, (2) considerar ceiling dinámico (70% del budget disponible) en lugar de número fijo. Decisión impacta TDR_003.

---

## Capa 3 — Fuentes Verificadas

| Fuente | URL | Fecha |
|:-------|:----|:------|
| Zylos — Context Window Management | https://zylos.ai/research/2026-01-19-llm-context-management/ | Jan 2026 |
| Zylos — Context Compression Strategies | https://zylos.ai/research/2026-02-28-ai-agent-context-compression-strategies/ | Feb 2026 |
| Zylos — Session Lifecycle Long-Running Agents | https://zylos.ai/research/2026-03-31-context-window-management-session-lifecycle-long-running-agents/ | Mar 2026 |
| Atlan — LLM Context Window Limitations | https://atlan.com/know/llm-context-window-limitations/ | Apr 2026 |
| Redis — Context Window Overflow | https://redis.io/blog/context-window-overflow/ | Jun 2026 |
| Morph — LLM Context Window Comparison | https://www.morphllm.com/llm-context-window-comparison | Jun 2026 |
