# Ontología de Validación — Nivel Business
## SDLC Governance Framework v1.0.0
**Fecha de captura:** 2026-06-14
**Auditor:** Tech Lead / Evidence Collection Pass
**Método:** Web search + paper review contra decisiones BDR/BREQ principales

---

## HALLAZGO CRÍTICO — BMAD Method: El competidor más cercano

### Qué es BMAD
BMAD-METHOD (Build More Architect Dreams) es un framework open-source MIT que orquesta 12+ agentes AI especializados a través del SDLC completo. Versión 6.6.0 con ~49,000 GitHub stars y 5,700 forks a junio 2026.

BMAD define roles especializados (Business Analyst, PM, Architect, Scrum Master, Developer, QA) como system prompts detallados que pasan artefactos estructurados de fase en fase — cada agente lee el output del anterior y escribe el suyo, manteniendo una cadena trazable desde requerimientos hasta PR.

### Convergencia con nuestras decisiones
Esto es significativo: llegamos independientemente a decisiones muy similares a las que BMAD tomó y que el mercado ha validado con ~49K stars.

| Concepto | BMAD | Nuestro Framework | Convergencia |
|:---------|:-----|:-----------------|:-------------|
| Agentes especializados por rol | BA, PM, Architect, Scrum Master, Dev, QA | Business Catalyst, PM Orchestrator, System Architect, Tech Lead, Git Butler, Librarian | ✅ Alta |
| Artefactos estructurados por fase | PRD, Architecture doc, Stories | BDRs, BREQs, PDRs, PREQs, ADRs, TDRs | ✅ Equivalente |
| Secuencia antes que código | Front-loaded planning | Phase 1 (planning) → Phase 2 (execution) | ✅ Idéntico |
| HITL en cada paso | Human refinement loops | HITL sign-off + cooling-off transitions | ✅ Análogo |
| Git como backbone | Versioned artifacts en repo | Git como fuente de verdad (ADR_002) | ✅ Idéntico |
| Audit trail | "Continuous compliance ledger" | Ledger de eventos + GIR_001 | ✅ Análogo |

### Dónde somos fundamentalmente distintos

**1. Jerarquía de gobernanza estructurada**
BMAD produce PRDs y Architecture docs como documentos de texto narrativo. Nosotros producimos una jerarquía formal y trazable BDR→BREQ→PDR→PREQ→ADR→AREQ→TDR→TREQ→DEV→TEST con relaciones parent_id, cross_plane y depends_on explícitas. BMAD es un "process multiplier, not a process creator" — si tu equipo ya piensa en PRDs y architecture docs, BMAD lo acelera y lo hace auditable. Nosotros construimos el proceso mismo, con la jerarquía y las reglas de integridad integradas.

**2. Auditoría de integridad formal (GIR)**
BMAD no tiene equivalente al GIR_001 — un motor que verifica activamente que todas las relaciones entre artefactos son correctas, que cada decisión está trazada hasta un BDR, que no hay referencias colgantes. BMAD en su forma actual delega responsabilidades críticas de decisión a usuarios no entrenados, careciendo de controles de calidad para prevenir que los agentes de desarrollo omitan los controles. Nuestro GIR_001 existe precisamente para cerrar este gap.

**3. Compliance con EU AI Act**
BMAD menciona SOC 2 / HIPAA en marketing pero no tiene evidencia de un modelo de HITL estructurado que cumpla EU AI Act Article 14 (context + authority + rationale documentados por decisión). Nuestro cooling-off + sign-off explícito + Ledger sí lo hace.

**4. Ingestion Compliance Protocol**
Las ICDs (Ingestion Compliance Declarations) entre capas no tienen equivalente en BMAD — el framework no verifica formalmente que todos los artefactos de una capa fueron procesados antes de pasar a la siguiente.

**5. Multi-proyecto / Project Registry**
BMAD opera proyecto por proyecto sin un registry formal de proyectos ni switch entre proyectos (ADR_005). Nuestro Project Registry con flush-before-load gestiona múltiples proyectos concurrentes.

### Los gaps de BMAD que nosotros resolvemos
Análisis de gaps estructurales de BMAD v6: el método delega decisiones críticas a usuarios no entrenados, carece de controles de calidad para prevenir que los agentes omitan los controles, e invalida la premisa básica de ser accesible a todos.

Esto confirma que nuestro GIR_001 + Ingestion Compliance Protocol + jerarquía formal de artefactos son diferenciadores reales, no overhead.

### Entrada ontológica
```yaml
concept: "bmad-method-primary-competitor-reference"
captured_date: "2026-06-14"
sources:
  - {id: "bmad-method-github", type: "open-source", title: "BMAD-METHOD v6.6.0 — 49K stars, MIT license", url: "https://github.com/bmad-code-org/BMAD-METHOD", stars: 49000, year: 2026}
  - {id: "bmad-gaps-issue-2003", type: "community-analysis", title: "Structural Gaps and Contradictions of BMAD v6 — GitHub Issue #2003", url: "https://github.com/bmad-code-org/BMAD-METHOD/issues/2003", year: 2026}
  - {id: "bmad-applied-2025", type: "case-study", title: "Applied BMAD — Reclaiming Control in AI Development", url: "https://bennycheung.github.io/bmad-reclaiming-control-in-ai-dev", year: 2025}
  - {id: "marktechpost-sdd-2026", type: "market-analysis", title: "9 Best AI Tools for Spec-Driven Development in 2026", url: "https://www.marktechpost.com/2026/05/08/9-best-ai-tools-for-spec-driven-development-in-2026-kiro-bmad-gsd-and-more-compare/", year: 2026}
informs_artifacts: ["BDR_001", "PDR_001", "ADR_001", "TDR_015"]
confidence: HIGH
stale_after_months: 3
injected_by: "agent"
finding: >
  BMAD is the closest market equivalent to our framework (~49K GitHub stars, validated by market).
  Key differentiators we have that BMAD lacks:
  (1) Formal artifact hierarchy with explicit traceability (BDR→DEV chain vs narrative docs)
  (2) GIR_001 integrity audit engine — automated verification BMAD explicitly lacks
  (3) Ingestion Compliance Protocol between layers
  (4) EU AI Act Article 14 compliant HITL model
  (5) Multi-project Registry with state isolation
  Competitive positioning: we are BMAD + formal governance enforcement + regulatory compliance.
```

---

## DECISIÓN 1 — BDR_005: Human-Centric Expert Assistant Model

### Lo que decidimos
El framework no reemplaza el juicio humano — asiste a expertos humanos. Agentes como colaboradores, no decisores. HITL retiene autoridad final.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| Natarajan et al. 2025 — "AI-in-the-Loop (AI2L)" | Paper académico | ✅ CONFIRMA exactamente — "humans decide, AI assists" |
| EU AI Act Article 14 — human oversight | Regulación | ✅ CONFIRMA — oversight es requisito legal, no opcional |
| HITL Orchestration paper 2026 — tiered patterns | Paper | ✅ CONFIRMA — tiered HITL con gates en decisiones críticas |
| Forrester 2026 — "balance automation with human oversight" | Analista | ✅ CONFIRMA — Gartner y Forrester alineados |
| PwC 2026 — "governance, measurement, human-AI collaboration = core design principles" | Consultora | ✅ CONFIRMA |

### Hallazgo — "AI2L" como término emergente
Natarajan et al. 2025 reenmarca la discusión con el concepto de AI-in-the-Loop (AI2L) — donde humanos, no AI, permanecen como tomadores de decisiones. Argumentan que esta distinción es crítica para diseñar sistemas que enfatizan colaboración sobre automatización. Esto valida BDR_005 a nivel conceptual — somos AI2L, no HITL tradicional donde el humano es un aprobador pasivo.

### Veredicto
**✅ CONFIRMA BDR_005.** El modelo human-centric es el estado del arte académico, regulatorio, y de industria.

### Entrada ontológica
```yaml
concept: "human-centric-ai-assisted-decision-model-ai2l"
captured_date: "2026-06-14"
sources:
  - {id: "ai2l-natarajan-2025", type: "paper", title: "AI-in-the-Loop: reframing HITL — humans decide, AI assists", year: 2025}
  - {id: "eu-ai-act-art14", type: "regulation", title: "EU AI Act Article 14 — Human Oversight: context, authority, rationale"}
  - {id: "pwc-agentic-sdlc-2026", type: "industry", title: "PwC — Agentic SDLC: governance + human-AI collaboration as core design principles", year: 2026}
  - {id: "forrester-agentic-2026", type: "analyst", title: "Forrester — Agentic SDLC: balance automation with human oversight", year: 2026}
informs_artifacts: ["BDR_005", "BDR_018", "ADR_003"]
confidence: HIGH
stale_after_months: 24
injected_by: "agent"
finding: "CONFIRMS BDR_005. AI2L > HITL as framing. EU AI Act Article 14 compliance is competitive differentiator."
```

---

## DECISIÓN 2 — BDR_001 / Jerarquía de 4 capas (Business→Product→Architecture→Technical)

### Lo que decidimos
Cuatro capas de gobernanza con artefactos propios, ICDs entre capas, y responsabilidades de agentes por capa.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| MetaGPT — "Product Manager, Architect, PM, Engineer, QA" secuencial | Paper académico (ACL 2024) | ✅ CONFIRMA: role specialization en SDLC es validado por investigación |
| RTADev — 5 agentes secuenciales (PM, Architect, PM, Programmer, Test) | Paper académico (ACL 2025) | ✅ CONFIRMA: secuencia definida reduce misalignment |
| ADLC (EPAM 2026) — Phase 0: human-agent responsibility mapping | Framework | ✅ CONFIRMA: separación de responsabilidades en capas es necesaria |
| BMAD — 12+ personas especializadas secuenciales | Open source validado | ✅ CONFIRMA mercado |

### Hallazgo — MetaGPT y RTADev
MetaGPT define 5 roles en su "software company": Product Manager, Architect, Project Manager, Engineer, y QA Engineer, trabajando secuencialmente con SOPs documentados. El Product Manager produce un PRD detallado con User Stories que sirve como breakdown funcional para los siguientes roles.

RTADev identifica un problema crítico: "misalignment durante el proceso de desarrollo" — el Project Manager malentiende un requerimiento y escribe una descripción de función incorrecta, lo que lleva al programador a asignar mal el trabajo. Su solución: "intention alignment" en cada handoff entre agentes. Esto valida exactamente nuestro Ingestion Compliance Protocol — los ICDs son el mecanismo de "intention alignment" entre capas.

### Veredicto
**✅ CONFIRMA BDR_001 / jerarquía de 4 capas.** La secuencia de roles especializados con artefactos de handoff es el estado del arte. Los ICDs son nuestra solución al "misalignment problem" documentado en la literatura.

### Entrada ontológica
```yaml
concept: "multi-layer-specialized-agent-sequential-sdlc-with-handoff-artifacts"
captured_date: "2026-06-14"
sources:
  - {id: "metagpt-2023", type: "paper", title: "MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework", url: "https://arxiv.org/abs/2308.00352", year: 2023}
  - {id: "rtadev-acl2025", type: "paper", title: "RTADev: Intention Aligned Multi-Agent Framework for Software — ACL 2025", url: "https://aclanthology.org/2025.findings-acl.80.pdf", year: 2025}
  - {id: "adlc-epam-2026", type: "framework", title: "ADLC: Agentic Development Lifecycle — human-agent responsibility mapping", url: "https://www.epam.com/insights/ai/blogs/agentic-development-lifecycle-explained", year: 2026}
informs_artifacts: ["BDR_001", "PREQ_005", "TDR_014"]
confidence: HIGH
stale_after_months: 18
injected_by: "agent"
finding: "CONFIRMS 4-layer hierarchy. ICDs validated as solution to 'misalignment problem' documented in RTADev (ACL 2025)."
```

---

## DECISIÓN 3 — BDR_002/BREQ_001-006: Business Catalyst + Elicitación estructurada

### Lo que decidimos
Business Catalyst como primer agente, elicitación guiada de requerimientos via entrevista estructurada, distinguiendo restricciones técnicas de preferencias.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| arxiv 2409.00038 — "AI Multi-agent for Requirements Elicitation" (2024) | Paper | ✅ LLMs para RE es área activa de investigación |
| Akhtar & Akhtar 2025 — "Requirements Elicitation in Transition" | Paper | ✅ AI/NLP para extracción automática es práctica emergente |
| ScopeMaster — "3-tiered approach: Objectives→Capabilities→Functional" | Industria | ✅ Jerarquía de reqs similar a BDR→BREQ |
| Forrester/Gartner 2026 — RE como primera fase del agentic SDLC | Analistas | ✅ |

### Hallazgo — Investigación de RE multi-agente
Investigación empírica (arxiv 2409.00038) sobre el uso de LLMs para automatizar análisis de requerimientos: sistema multi-agente que genera user stories desde requerimientos iniciales, evalúa y mejora su calidad, y las prioriza. Evalúa GPT-3.5, GPT-4, LLaMA3-70, y Mixtral en 4 proyectos reales. El resultado: la calidad mejora significativamente con múltiples modelos colaborando vs. uno solo — mismo principio que nuestro pipeline de 4 capas.

### Veredicto
**✅ CONFIRMA BREQ_001-006.** Elicitación estructurada con AI es el estado del arte. La distinción restricción/preferencia (BREQ_001) está bien fundamentada en la literatura de RE.

---

## DECISIÓN 4 — BDR_018/BDR_020: Governance Integrity Audit + Non-Destructive Operations

### Lo que decidimos
GIR_001 como auditoría formal de 5 planos sobre el artifact graph. Operaciones no-destructivas con archivado, no borrado.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| elevateconsult.com 2026 — "Evidence Maps for AI Governance" | Industria | ✅ Evidence maps = artifact traceability maps = nuestra dependency graph |
| EU AI Act / NIST AI RMF / ISO 42001 | Regulación | ✅ Immutable audit logs son requisito regulatorio |
| BMAD Issue #2003 — falta de quality controls | Gap identificado | ✅ Valida GIR como diferenciador |

### Hallazgo — "Evidence Maps"
Evidence maps crean conexiones trazables entre obligaciones y artefactos. A diferencia de simples checklists, establecen relaciones dinámicas vinculando requerimientos regulatorios a resultados de pruebas, documentación y controles operativos específicos. Esto es exactamente lo que hace nuestro GIR_001 Plane 1-3: no verifica presencia de artefactos, verifica las RELACIONES entre ellos.

### Veredicto
**✅ CONFIRMA BDR_018/BDR_020.** GIR_001 está validado como práctica de governance de IA. Las operaciones no-destructivas (archivado vs. borrado) son un principio estándar de compliance.

---

## DECISIÓN 5 — Posicionamiento de mercado y timing

### Evidencia del mercado
| Fuente | Dato | Implicación |
|:-------|:-----|:------------|
| Forrester Jun 2026 | Agentic SDLC: 2023-24 code, 2025 design/docs, 2026 end-to-end orchestration | ✅ Estamos en el momento exacto del mercado |
| Booz Allen 2026 | "Gartner: 90% of engineers will use AI code assistants by 2028" | ✅ Mercado masivo en 3 años |
| PwC 2026 | "governance, measurement, human-AI collaboration = core design principles" | ✅ Governance es el gap que el mercado busca |
| BMAD Issue #2003 | Framework líder tiene gaps estructurales de governance | ✅ Oportunidad real |
| ADLC EPAM 2026 | "Skipping Phase 0 pushes compliance, risk, accountability issues to production" | ✅ Valida nuestra Phase 1 completa |

### Hallazgo de timing
Forrester, que acuñó "TuringBots" en 2021, señala que el mercado está cambiando hacia sistemas agenticos que coordinan a través del SDLC completo. Describe una evolución multi-año: code assistants 2023-2024, soporte a diseño y testing en 2025, y automatización end-to-end orquestada en 2026.

Estamos construyendo exactamente lo que Forrester dice que el mercado está adoptando en 2026. El timing es correcto.

### Hallazgo de diferenciación
Para que los agentes produzcan output confiable a escala enterprise, cuatro capacidades deben estar presentes: contexto (operating picture completo), conocimiento (memoria viva de cómo trabaja el equipo), colaboración multi-player, y gobernanza (scoped access, attributed runs, guardrails auditables).

El framework cubre las cuatro. Especialmente la cuarta — governance — que es donde BMAD tiene sus gaps más documentados.

### Entrada ontológica
```yaml
concept: "agentic-sdlc-market-timing-2026-governance-gap"
captured_date: "2026-06-14"
sources:
  - {id: "forrester-agentic-jun2026", type: "analyst", title: "Forrester: Agentic SDLC market evolution 2023-2026", year: 2026}
  - {id: "booz-allen-gartner-2026", type: "analyst", title: "Gartner: 90% of engineers using AI code assistants by 2028", year: 2026}
  - {id: "pwc-governance-2026", type: "industry", title: "PwC: governance + human-AI collaboration as core agentic SDLC design principles", year: 2026}
  - {id: "coderabbit-agentic-2026", type: "industry", title: "Agentic SDLC: 4 required capabilities including governance with auditable guardrails", year: 2026}
informs_artifacts: ["BDR_001", "PDR_001", "BREQ_001"]
confidence: HIGH
stale_after_months: 6
injected_by: "agent"
finding: >
  Timing is correct: Forrester confirms 2026 is the year of end-to-end orchestrated agentic SDLC.
  Governance gap is the market opportunity: BMAD (49K stars) has documented structural governance gaps.
  Our framework addresses all 4 capabilities CodeRabbit identifies as required for enterprise-grade agentic SDLC.
  Competitive positioning: 'BMAD + formal governance enforcement' = our v1.0 value proposition.
```

---

## RESUMEN EJECUTIVO — Nivel Business

### Scorecard de decisiones Business

| Decisión | BDR/BREQ | Veredicto | Observación |
|:---------|:---------|:----------|:------------|
| Human-centric model (BDR_005) | BDR_005 | ✅ CONFIRMADO | AI2L > HITL como framing. EU AI Act compliance = diferenciador |
| Jerarquía 4 capas (BDR_001) | BDR_001 | ✅ CONFIRMADO | RTADev ACL 2025 valida ICDs como solución al misalignment problem |
| Business Catalyst + elicitación | BREQ_001-006 | ✅ CONFIRMADO | RE multi-agente validado por investigación |
| GIR + Non-Destructive | BDR_018/020 | ✅ CONFIRMADO | Evidence maps = nuestro artifact traceability. Requisito regulatorio |
| Timing de mercado | PDR_001 | ✅ CONFIRMADO | Forrester: 2026 es el año del agentic SDLC end-to-end |

### El hallazgo más importante: BMAD
**BMAD es nuestra referencia de mercado más directa.** 49,000 stars, MIT, activo. Converge con nosotros en la mayoría de las decisiones fundamentales — esto es validación de que el approach es correcto.

**Nuestros diferenciadores reales sobre BMAD:**
1. Jerarquía formal BDR→DEV con traceability completa (vs. documentos narrativos)
2. GIR_001 — auditoría de integridad automatizada (BMAD Issue #2003 documenta este gap)
3. Ingestion Compliance Protocol entre capas (vs. handoffs informales)
4. EU AI Act Article 14 compliant HITL model (vs. HITL superficial)
5. Multi-proyecto con state isolation (ADR_005)
6. Ontología versionada con evidencia (BPROP_001 — BMAD no tiene nada similar)

**Propuesta de valor sintetizada:**
"Lo que BMAD hace para el developer promedio, nosotros lo hacemos con governance formal, compliance regulatorio, y evidencia verificable — para el mercado que necesita más que un audit trail de marketing."

