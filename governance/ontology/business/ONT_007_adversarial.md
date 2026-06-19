---
id: ONT_007
concept: "human-centric-ai-assisted-decision-model-ai2l"
layer: "business"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "HIGH"
stale_after_months: 12
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_007_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada y búsqueda adversarial per TREQ_045."
---

# ONT_007 — Human-Centric AI2L Model
## Digest Adversarial Verificado — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
Esta decisión estaría equivocada si el overhead de HITL en cada
decisión hace que el developer abandone el framework antes del
primer milestone, o si el mercado prefiere autonomía completa del
agente sobre governance controlada.

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor
**Elementum AI — HITL Agentic AI (Mar 2026) — elementum.ai:**
"Gartner projects 70% of enterprises will deploy agentic AI as part
of IT infrastructure by 2029." Crucial: "a poorly trained reviewer
approving flawed agent outputs is worse than no checkpoint at all."
→ HITL bien diseñado es el diferenciador — HITL superficial es peor
que no tener HITL.

**PwC Agentic SDLC 2026 — pwc.com:**
"governance, measurement and human-AI collaboration become core
design principles." 70% de software teams usan GenAI en niveles
moderados o altos across SDLC. EU AI Act y California SB-833
(efectivo julio 2026) hacen HITL documentado un requisito legal,
no una opción.

**Multi-agent LLM for Requirements Elicitation — ACL 2026:**
Investigación empírica: multi-agent RE con HITL produce user stories
de mayor calidad que single-agent sin revisión humana. Elicitron
y MARE frameworks usan HITL como gate de calidad explícito.

### B — Evidencia en Contra y Riesgos
**SurveyMonkey CX 2025 (citado en elementum.ai):**
79% prefieren interactuar con humano sobre AI para servicio complejo.
→ El HITL no es el problema — el problema es HITL mal implementado
que se siente como burocracia. Si el cooling-off se percibe como
interrupciones sin valor, el developer lo desactiva.

**DronaHQ Agentic SDLC 2026:**
"In an agentic SDLC, small, focused agents take on full tasks within
trusted boundaries." El mercado está moviendo hacia autonomía mayor
de los agentes, no hacia más gates. La tendencia es reducir HITL
en tareas rutinarias.

**BayTech Consulting — Skill Atrophy risk:**
"Over-reliance on AI can lead to decline in developers' problem-solving
skills." El HITL que simplemente aprueba sin entender es un riesgo
— no solo de calidad sino de desarrollo profesional del operador.

### C — Alternativas Laterales
- Async HITL: agentes trabajan en background, HITL aprueba en batch
  en lugar de bloquearse en cada decisión. Reduce interrupciones
  sin comprometer governance.
- Tiered HITL: decisions críticas (BDRs, ADRs) requieren HITL,
  decisions rutinarias (formateo, IDs, slugs) se auto-aprueban.
  Framework Profiles (BDR_021) ya implementa esto parcialmente.

---

## FASE 2 — Síntesis con Tensión
El HITL bien diseñado es lo que el mercado enterprise exige y la
regulación requiere. Pero HITL mal diseñado — que se siente como
interrupciones sin valor — activamente daña la adopción. El riesgo
no es el principio sino la UX de su implementación.

**Veredicto:** CONFIRMADA_CON_RIESGO
El modelo AI2L con HITL es correcto y regulatoriamente necesario.
Riesgo: la UX del cooling-off y sign-off debe ser diseñada para
comunicar valor en cada interrupción — si el developer no entiende
por qué está aprobando, el HITL se vuelve teatro.

**Fuentes verificadas:**
| elementum.ai HITL | https://www.elementum.ai/blog/human-in-the-loop-agentic-ai | Mar 2026 |
| PwC Agentic SDLC | https://www.pwc.com/m1/en/publications/2026/docs/future-of-solutions-dev-and-delivery-in-the-rise-of-gen-ai.pdf | Jan 2026 |
| ACL 2026 Multi-agent RE | https://dl.acm.org/doi/10.1007/978-3-031-78386-9_20 | 2026 |
| DronaHQ Agentic SDLC | https://www.dronahq.com/agentic-sdlc-guide/ | May 2026 |
