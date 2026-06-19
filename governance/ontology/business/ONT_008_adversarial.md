---
id: ONT_008
concept: "multi-layer-specialized-agent-sequential-sdlc-handoff-artifacts"
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
    document_id: "ONT_008_original"
    link_reason: "Revisión adversarial con evidencia verificada 2026."
---

# ONT_008 — Multi-Layer Specialized Agents with Handoff Artifacts
## Digest Adversarial Verificado — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
Esta decisión estaría equivocada si la especialización de agentes
por capa produce fricción de handoff que supera el beneficio de la
separación de responsabilidades.

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor
**ACL 2025 — Multi-agent LLM Requirements Analysis (arxiv/dl.acm.org):**
"Multi-agent LLM system for automated requirements analysis: user story
generation and prioritization." Investigación empírica sobre múltiples
proyectos reales. Agentes especializados mejoran calidad de RE vs
agente único.

**Microsoft Community Hub — AI-Led SDLC (Feb 2026):**
"GitHub Spec Kit: placing specification at centre of engineering process.
Specs drive implementation, checklists, task breakdowns." AI code reviews
aumentaron quality improvements de 55% a 81% (Qodo 2025). Atlassian
RovoDev 2026: 38.7% de comentarios de AI agents en code reviews
producen fixes adicionales.

**arxiv — LLM Multi-Agent Systems for SE (Dec 2025):**
"Multi-agent LMA systems enable autonomous problem-solving, improve
robustness, scalable solutions for managing complexity of real-world
software projects." MARE framework cubre elicitación, modelado,
verificación y especificación — validación académica del approach
multi-layer.

### B — Evidencia en Contra y Riesgos
**baytechconsulting.com — Agentic SDLC 2025:**
"AI is collapsing distinct phases of requirements, design, and development
into a highly compressed, iterative super-phase of Design & Experiment."
Hay una tendencia de mercado hacia compresión de fases, no hacia
separación rígida. Nuestro modelo de 4 capas va en dirección contraria.

**gurusup.com — Multi-Agent Frameworks 2026:**
"Building coordination primitives from scratch means reinventing message
passing, state checkpointing, handoff protocols." El handoff entre capas
tiene costo de implementación real — no es trivial hacerlo bien.

### C — Alternativas Laterales
- "Design & Experiment" super-phase: comprimir Business + Product en
  una iteración rápida (2-4 horas) antes de separar Architecture y
  Technical. Reduce fricción de handoff manteniendo separación en
  las capas más técnicas donde más importa.
- Agentes paralelos DENTRO de una capa con batch sign-off: ya
  considerado en ONT_005. Reduce tiempo sin comprometer governance.

---

## FASE 2 — Síntesis con Tensión
La separación de responsabilidades por capas es correcta y validada
académicamente. La tensión es que el mercado está comprimiendo fases
— especialmente Business y Product que pueden colapsar en iteraciones
muy rápidas. Framework Profiles (BDR_021) PROFILE_LITE ya responde
a esto al hacer las capas Business y Product opcionales/simplificadas.

**Veredicto:** CONFIRMADA
La separación multicapa está bien fundamentada. PROFILE_LITE (BDR_021)
mitiga el riesgo de sobre-rigidez al permitir comprimir las capas
superiores cuando el proyecto no las necesita completas.

**Fuentes verificadas:**
| ACL Multi-agent RE | https://dl.acm.org/doi/10.1007/978-3-031-78386-9_20 | 2026 |
| Microsoft AI-Led SDLC | https://techcommunity.microsoft.com/blog/appsonazureblog/an-ai-led-sdlc-building-an-end-to-end-agentic-software-development-lifecycle-wit/4491896 | Feb 2026 |
| arxiv LLM Multi-Agent SE | https://arxiv.org/html/2404.04834v4 | Dec 2025 |
| BayTech Agentic SDLC | https://www.baytechconsulting.com/blog/agentic-sdlc-ai-software-blueprint | Aug 2025 |
