# Ontología de Validación — Nivel Architecture
## SDLC Governance Framework v1.0.0
**Fecha de captura:** 2026-06-14  
**Auditor:** Tech Lead / GIR Engine (Evidence Collection Pass)  
**Método:** Web search + paper review contra decisiones ADR_001–ADR_006

---

## DECISIÓN 1 — ADR_001: Adapter Pattern / Vendor-Agnostic Abstraction

### Lo que decidimos
ABC con `AdapterResult`, implementaciones intercambiables por proveedor, registro vía `.butler.env`.

### Evidencia encontrada
| Fuente | Tipo | Relevancia | Veredicto |
|:-------|:-----|:-----------|:----------|
| USPTO Patent 12405978 — "Adapter pattern for LLM agents" (2025) | Paper/Patent | Alta | ✅ Confirma: el adapter pattern está formalmente reconocido como el patrón estándar para I/O de LLM agents |
| MCP (Model Context Protocol) — Anthropic, Nov 2024 | Estándar abierto | Crítica | ⚠️ RIESGO: ver análisis abajo |
| A2A Protocol (Google + 50 partners) — 2025 | Estándar abierto | Media | ⚠️ Ecosistema moviéndose hacia protocolos estándar |
| Quiq Blog — "LLM Agnostic Approach" (Mar 2025) | Industria | Media | ✅ Confirma: abstraction layer es best practice documentada |

### Hallazgo crítico — MCP
**MCP (Model Context Protocol)** es el estándar emergente dominante para exactamente el problema que resuelve nuestro `AdapterResult`: conectar LLMs con herramientas/servicios externos de forma vendor-neutral. Adoptado por Google, Microsoft, OpenAI y 50+ partners. Es JSON-RPC sobre stdio/HTTP. Nuestro adapter pattern reinventa parte de esto pero de forma más acotada (solo Git + Filesystem + LLM, no un protocolo general). 

**Implicación para el framework:** En v1.0 nuestro scope es correcto (no necesitamos un protocolo general para 3 adapters). Pero si el framework crece hacia multi-herramienta en v2.0+, MCP es el camino natural en lugar de continuar extendiendo el ABC artesanal.

### Veredicto
**✅ CONFIRMA ADR_001 para v1.0.** El adapter pattern es el estado del arte. La única consideración es que MCP existe como estándar más general — añadir a la ontología como "Future Horizon: evaluar migración a MCP en v2.0+" en lugar de extender el ABC.

### Entrada ontológica
```yaml
concept: "vendor-agnostic-adapter-pattern-for-llm-agents"
captured_date: "2026-06-14"
sources:
  - {id: "uspto-12405978", type: "patent", title: "Adapter pattern for LLM agents I/O transformation", year: 2025}
  - {id: "mcp-spec-2024", type: "open-standard", title: "Model Context Protocol — Anthropic", url: "https://spec.modelcontextprotocol.io", year: 2024}
  - {id: "a2a-google-2025", type: "open-standard", title: "Agent-to-Agent Protocol — Google + 50 partners", year: 2025}
informs_artifacts: ["ADR_001", "AREQ_002"]
confidence: HIGH
stale_after_months: 12
injected_by: "agent"
finding: "CONFIRMS — with Future Horizon: evaluate MCP migration for v2.0+ multi-tool scope"
```

---

## DECISIÓN 2 — ADR_002: Git como backbone + Bootstrap Protocol (BREQ_017)

### Lo que decidimos
Git como única fuente de verdad, bootstrap vía GitAdapter (GitHub v1.0), `.butler.env` para configuración, Path A/B/C/D para diferentes estados del repo.

### Evidencia encontrada
| Fuente | Tipo | Relevancia | Veredicto |
|:-------|:-----|:-----------|:----------|
| ADR GitHub (adr.github.io) — comunidad activa 2024-2026 | Comunidad establecida | Alta | ✅ Git-based ADRs es práctica estándar de industria |
| MADR 4.0.0 (Sep 2024) — Markdown ADR formato estándar | Estándar | Alta | ✅ Markdown en Git para decisiones arquitectónicas es el estado del arte |
| joelparkerhenderson/architecture-decision-record | Open Source | Alta | ✅ ADR + fitness functions en CI = exactamente lo que tenemos con GIR_001 |
| Azure Well-Architected Framework — ADRs (Nov 2024) | Enterprise | Alta | ✅ Microsoft adopta ADRs en su framework enterprise |
| adr-tooling (adr.github.io) — log4brains, adr-tools | Tooling | Media | ⚠️ Existe tooling maduro que no conocíamos |

### Hallazgo — Tooling existente
Existe ecosistema maduro de ADR tooling: **log4brains** (gestión + publicación), **adr-tools** (bash scripts Nygard format), **MADR CLI**, **Talo** (CLI para ADRs, RFCs, docs). El framework construye su propio Librarian para gestionar artefactos de gobernanza cuando parte de este problema ya está resuelto para el caso específico de ADRs.

**Implicación:** Para el framework mismo (dogfooding), estas herramientas no aplican porque tenemos una jerarquía de artefactos más compleja (BDR→BREQ→PDR→TDR etc.) que ningún ADR tool gestiona. Para el *output* que el framework produce en proyectos de usuarios — si el usuario solo quiere ADRs, podría preferir log4brains sobre la estructura completa. Esto no invalida la decisión, pero es un posible punto de diferenciación a comunicar.

### Hallazgo — Fitness Functions
El repo de joelparkerhenderson menciona explícitamente "fitness functions for decisions" como práctica para governance de ADRs — checks automatizados en CI que verifican que las decisiones se cumplen. Esto es exactamente lo que GIR_001 hace, validado independientemente.

### Veredicto
**✅ CONFIRMA ADR_002.** Git es la elección correcta y bien establecida. ADR tooling existente no aplica a nuestra jerarquía compleja pero vale documentarlo. Fitness functions en CI = GIR_001 validado por la comunidad.

### Entrada ontológica
```yaml
concept: "git-as-governance-backbone-with-decision-records"
captured_date: "2026-06-14"
sources:
  - {id: "adr-github-io", type: "community-standard", title: "ADR GitHub Community + Tooling Ecosystem", url: "https://adr.github.io", year: 2024}
  - {id: "madr-4.0", type: "standard", title: "Markdown Architectural Decision Records 4.0.0", year: 2024}
  - {id: "joelparkerhenderson-adr", type: "open-source", title: "ADR + Fitness Functions pattern", url: "https://github.com/joelparkerhenderson/architecture-decision-record"}
  - {id: "azure-well-architected-2024", type: "enterprise-framework", title: "Azure Well-Architected Framework — ADRs", year: 2024}
informs_artifacts: ["ADR_002", "BREQ_017", "TDR_001"]
confidence: HIGH
stale_after_months: 24
injected_by: "agent"
finding: "CONFIRMS — Fitness functions in CI (GIR_001) independently validated by ADR community"
```

---

## DECISIÓN 3 — ADR_003: Context Distillation + 9,500-token hard ceiling

### Lo que decidimos
Patrón de destilación de contexto con un ceiling fijo de 9,500 tokens, técnica manual de resumen estructurado, "active artifact slot" + "session summary".

### Evidencia encontrada
| Fuente | Tipo | Relevancia | Veredicto |
|:-------|:-----|:-----------|:----------|
| ContextBudget paper (arxiv 2604.01664) — 2026 | Paper académico | Crítica | ✅ Budget-aware context management es área de investigación activa |
| MatClaw — "caps effective context at 200K for 1M models" (2025) | Paper | Alta | ✅ Confirma que los ceilings conservadores son práctica correcta |
| RULER/HELMET benchmarks (2024) — accuracy drops before 32K | Benchmark | Alta | ⚠️ RIESGO CRÍTICO: ver análisis abajo |
| LLMLingua — Microsoft Research, token-level compression | Tool | Media | ⚠️ Herramienta existente que no evaluamos |
| Acon — 26-54% memory reduction preserving 95% performance | Paper | Alta | ⚠️ Técnicas de compresión más sofisticadas disponibles |
| "Context rot" — Hong et al. 2025 | Paper | Alta | ✅ Confirma nuestro enfoque conservador |

### Hallazgo crítico — El 9,500-token ceiling
La investigación de 2024-2025 establece que "context rot" existe: rendimiento degrada antes del límite técnico del contexto. RULER/HELMET benchmarks muestran que GPT-4o cae de near-perfect a high-60s en 32K tokens. MatClaw usa 200K como ceiling operativo para modelos que anuncian 1M tokens. 

**Nuestra decisión de 9,500 tokens está bien fundamentada** — es un ceiling conservador que refleja exactamente lo que la literatura documenta. Sin embargo hay un matiz importante: **la investigación 2025-2026 muestra que simple observation masking puede ser igual de efectivo que LLM summarization** para context management (paper "The Complexity Trap"). Esto sugiere que nuestro enfoque de destilación LLM-assisted podría ser más costoso de lo necesario.

### Hallazgo — LLMLingua y técnicas de compresión
Microsoft Research tiene LLMLingua (open source) que hace token-level pruning guiado por señales del modelo — 65-80% de reducción de tokens sin pérdida significativa de información. No lo evaluamos. Podría ser relevante para optimizar el costo del contexto en sesiones largas.

### Veredicto
**✅ CONFIRMA ADR_003 en principio, con una observación.** El 9,500-token ceiling es correcto. El patrón de destilación es válido. La observación: en v2.0 vale evaluar si LLMLingua o técnicas de observation masking pueden hacer el trabajo más barato que destilación LLM-assisted completa.

### Entrada ontológica
```yaml
concept: "conservative-token-budget-context-distillation-llm-agents"
captured_date: "2026-06-14"
sources:
  - {id: "contextbudget-2026", type: "paper", title: "ContextBudget: Budget-Aware Context Management for Long-Horizon Agents", url: "https://arxiv.org/abs/2604.01664", year: 2026}
  - {id: "ruler-helmet-2024", type: "benchmark", title: "RULER + HELMET: LLM accuracy drops before 32K tokens in adversarial tasks", year: 2024}
  - {id: "context-rot-2025", type: "paper", title: "Context Rot — performance degradation from input length", year: 2025}
  - {id: "llmlingua-microsoft", type: "tool", title: "LLMLingua — token-level prompt compression, Microsoft Research", url: "https://github.com/microsoft/LLMLingua"}
  - {id: "complexity-trap-2025", type: "paper", title: "The Complexity Trap: Simple Observation Masking as efficient as LLM Summarization", year: 2025}
informs_artifacts: ["ADR_003", "TDR_003"]
confidence: HIGH
stale_after_months: 12
injected_by: "agent"
finding: "CONFIRMS with observation: evaluate LLMLingua/observation-masking in v2.0 as cheaper alternative to full LLM-assisted distillation"
```

---

## DECISIÓN 4 — ADR_004: YAML-based State Machine con atomic writes

### Lo que decidimos
`state.yaml` como fuente de verdad del estado de sesión, atomic writes vía temp-then-rename, filesystem como persistence layer.

### Evidencia encontrada
| Fuente | Tipo | Relevancia | Veredicto |
|:-------|:-----|:-----------|:----------|
| fast.io — "YAML/JSON human-readable for agent state" (Feb 2026) | Industria | Alta | ✅ YAML/JSON para estado de agente es práctica documentada y recomendada |
| Temporal.io — "workflow-as-code > YAML state machines" (2025) | Herramienta enterprise | Media | ⚠️ RIESGO: ver análisis abajo |
| LangGraph — YAML state machine + graph nodes | Framework mayor | Alta | ✅ State machine approach es mainstream en orquestación de agentes |
| indium.tech — "7 State Persistence Strategies for AI Agents" (Mar 2026) | Industria | Alta | ✅ Checkpoint-restore model = exactamente lo que tenemos |
| spaceo.ai — "LangGraph: state machine con nodes, edges, conditional routing" | Framework | Alta | ✅ Nuestro approach es equivalente a LangGraph's state management |

### Hallazgo — Temporal.io y el argumento contra YAML state machines
Temporal argumenta que "workflow-as-code es mejor que YAML state machine definitions — cuando la lógica se vuelve no-trivial, los YAML DSLs explotan en complejidad." Este es un punto legítimo para casos enterprise con workflows de larga duración (días/semanas). 

**Para nuestro caso:** El estado de sesión del SDLC framework es bounded — una sesión de planeación tiene un lifecycle definido y no requiere durabilidad de meses. El operador puede retomar desde checkpoints (state.yaml). Temporal añadiría infraestructura (servidor Temporal) que viola explícitamente nuestro principio de "no additional infrastructure." El riesgo de Temporal es real para v3.0 Enterprise con múltiples proyectos concurrentes, no para v1.0 Developer Solo.

### Hallazgo — Consistencia con industria
LangGraph usa exactamente el mismo approach: YAML/JSON state como fuente de verdad, nodes como agentes, edges como transiciones. Nuestra implementación es conceptualmente equivalente sin el overhead del grafo (tenemos secuencia lineal de agentes, no un DAG complejo).

### Veredicto
**✅ CONFIRMA ADR_004 para v1.0.** YAML state + atomic writes es la elección correcta para el scope actual. Temporal/LangGraph-checkpointing es relevante para v3.0 Enterprise — añadir a Future Horizon.

### Entrada ontológica
```yaml
concept: "yaml-state-machine-atomic-writes-agent-session-persistence"
captured_date: "2026-06-14"
sources:
  - {id: "fastio-yaml-state-2026", type: "industry", title: "AI Agent Workflow State Persistence Best Practices 2026", year: 2026}
  - {id: "temporal-2025", type: "tool", title: "Temporal: Beyond State Machines for Reliable Distributed Applications", url: "https://temporal.io/blog/temporal-replaces-state-machines", year: 2025}
  - {id: "langgraph-state-2025", type: "framework", title: "LangGraph State Machine Architecture", year: 2025}
  - {id: "indium-7-strategies-2026", type: "industry", title: "7 State Persistence Strategies for Long-Running AI Agents", year: 2026}
informs_artifacts: ["ADR_004", "TDR_004", "TREQ_007"]
confidence: HIGH
stale_after_months: 18
injected_by: "agent"
finding: "CONFIRMS — Temporal relevant only for v3.0 Enterprise multi-project concurrent scope (Future Horizon)"
```

---

## DECISIÓN 5 — ADR_003 (Orchestration Model): Single active agent, cooling-off transitions

### Lo que decidimos
Un solo agente activo por turno, transiciones explícitas con Transition Summary, roles definidos por system prompts intercambiables.

### Evidencia encontrada
| Fuente | Tipo | Relevancia | Veredicto |
|:-------|:-----|:-----------|:----------|
| HITL Orchestration paper — IJCT 2026 | Paper académico | Crítica | ✅ HITL como design pattern con tiered orchestration — confirma nuestro approach |
| Natarajan et al. 2025 — "AI-in-the-Loop (AI2L)" | Paper académico | Alta | ✅ Confirma: humanos deciden, AI asiste — exactamente BDR_005 |
| EU AI Act Article 14 — human oversight required | Regulatorio | Alta | ✅ Nuestro HITL no es opcional, es regulatoriamente correcto |
| Amazon Bedrock Agents — HITL patterns | Enterprise | Alta | ✅ Confirms: synchronous approval gates = exactamente cooling-off transitions |
| AutoGen, CrewAI, LangGraph — multi-agent patterns | Frameworks | Alta | ⚠️ Estos permiten agentes concurrentes — ver análisis |

### Hallazgo — Multi-agent concurrente
AutoGen, CrewAI, y LangGraph permiten múltiples agentes concurrentes — scatter-gather, pipeline parallelism. Nuestra decisión de un solo agente activo es más conservadora.

**Justificación validada:** Para un SDLC governance pipeline los agentes representan ROLES con perspectivas distintas (Business, Product, Architecture, Technical) — la secuencia es el punto, no la paralelización. Un Business Catalyst y un Tech Lead no deberían trabajar simultáneamente sobre el mismo artefacto porque sus outputs son interdependientes y secuenciales por diseño. Además, el contexto compartido (state.yaml) haría el acceso concurrente problemático sin un orchestrator más complejo. La decisión es arquitectónicamente correcta para nuestro caso de uso.

### Hallazgo regulatorio
El EU AI Act Article 14 requiere "human oversight demonstrable, trained, measurable, and provable" para sistemas de alto riesgo. Nuestro modelo de cooling-off + sign-off explícito + Ledger de eventos cumple exactamente este criterio. Esto es un diferenciador competitivo real frente a frameworks que tienen HITL superficial.

### Veredicto
**✅ CONFIRMA ADR_003 (Orchestration Model).** Single active agent es la decisión correcta para SDLC sequential pipeline. El modelo HITL cumple EU AI Act Article 14 — diferenciador competitivo.

### Entrada ontológica
```yaml
concept: "single-active-agent-hitl-governance-sequential-pipeline"
captured_date: "2026-06-14"
sources:
  - {id: "hitl-orchestration-2026", type: "paper", title: "HITL Orchestration for Agentic Use Cases — tiered design patterns", url: "https://ijctjournal.org/wp-content/uploads/2026/04/Human-in-the-Loop-HITL-Orchestration-for-Agentic-Use-Cases.pdf", year: 2026}
  - {id: "ai2l-natarajan-2025", type: "paper", title: "AI-in-the-Loop: humans decide, AI assists", year: 2025}
  - {id: "eu-ai-act-article14", type: "regulation", title: "EU AI Act Article 14 — Human Oversight Requirements"}
  - {id: "amazon-bedrock-hitl", type: "enterprise", title: "Amazon Bedrock Agents — synchronous HITL approval gates", year: 2025}
informs_artifacts: ["ADR_003", "BDR_005", "TDR_006", "TREQ_012"]
confidence: HIGH
stale_after_months: 24
injected_by: "agent"
finding: "CONFIRMS — EU AI Act Article 14 compliance is a competitive differentiator. Single-agent sequential model correct for SDLC pipeline."
```

---

## DECISIÓN 6 — ADR_005 + ADR_006: Project Registry + Brownfield Onboarding

### Lo que decidimos
Registry como markdown table, switch con flush-before-load, brownfield via Project Analysis Report al momento de bootstrap.

### Evidencia encontrada
| Fuente | Tipo | Relevancia | Veredicto |
|:-------|:-----|:-----------|:----------|
| LangGraph — project switching + state isolation patterns | Framework | Alta | ✅ Flush-before-load es el pattern correcto para state isolation |
| Microsoft Agent Framework — "wrap agent creation in helper to isolate state" | Enterprise | Alta | ✅ Confirma: state isolation entre proyectos es una necesidad documentada |
| GitHub topics: brownfield onboarding, codebase analysis | Comunidad | Media | ✅ Path D approach (analizar antes de inicializar) es common sense documentado |

### Veredicto
**✅ CONFIRMA ADR_005 y ADR_006.** State isolation y brownfield analysis son decisiones correctas y bien fundamentadas.

---

## DECISIÓN 7 — AI Evaluation Harnesses (BPROP_001 relacionado)

### Evidencia encontrada (nueva, no teníamos ADR al respecto)
| Fuente | Tipo | Hallazgo clave |
|:-------|:-----|:---------------|
| DeepEval v4.0.3 — MIT license, pytest-native, agent metrics | Tool | ✅ Para Developer Solo: FREE, local, no infrastructure |
| Inspect AI v0.3.225 — UK AI Security Institute | Tool | ✅ Open source, gobierno respaldado, standardized |
| Braintrust — $800M valuation, CI/CD integration | Commercial | Para cuando haya team |
| Promptfoo — adquirido por OpenAI Mar 2026 | Tool | ⚠️ Vendor objectivity question post-adquisición |
| LangSmith — tied to LangChain | Tool | No aplica, no usamos LangChain |

### Hallazgo crítico — DeepEval para v1.0
**DeepEval** (MIT license, pytest-native, sin dependencias de cloud) es exactamente lo que necesitamos para el Developer Solo profile. Tiene métricas específicas para multi-step agent traces y validación de tool calls. Es compatible con pytest (que ya usamos via PytestRunnerAdapter). **No es reinventar la rueda — existe la rueda y es open source.**

### Propuesta concreta
Agregar DeepEval como dependencia de testing en v1.0.0 — no como infraestructura nueva sino como extensión natural del PytestRunnerAdapter (TREQ_039). Los TEST_TICKETs que actualmente dicen "Integration: verificar que el agente produce X dado Y" serían evaluaciones DeepEval, no revisión manual.

### Entrada ontológica
```yaml
concept: "llm-agent-behavioral-evaluation-harnesses"
captured_date: "2026-06-14"
sources:
  - {id: "deepeval-v4", type: "tool", title: "DeepEval v4.0.3 — MIT license, pytest-native, agent traces", url: "https://github.com/confident-ai/deepeval"}
  - {id: "inspect-ai-ukaisc", type: "tool", title: "Inspect AI v0.3.225 — UK AI Security Institute", url: "https://github.com/UKGovernmentBEIS/inspect_ai"}
  - {id: "braintrust-2026", type: "commercial-tool", title: "Braintrust — CI/CD eval integration, $800M valuation post Series B", year: 2026}
  - {id: "inference-net-comparison-2026", type: "analysis", title: "LLM Evaluation Tools Complete Comparison 2026", url: "https://inference.net/content/llm-evaluation-tools-comparison/", year: 2026}
informs_artifacts: ["TREQ_039", "TREQ_040", "BPROP_001"]
confidence: HIGH
stale_after_months: 6
injected_by: "agent"
finding: "NEW PROPOSAL — DeepEval (MIT, pytest-native) should be added as v1.0 test dependency. Replaces manual agent behavior review in integration TEST_TICKETs. Braintrust for team scale in v2.0+."
```

---

## DECISIÓN 8 — Ontología como concepto (BPROP_001)

### Evidencia encontrada
| Fuente | Tipo | Hallazgo clave |
|:-------|:-----|:---------------|
| LLM-Driven Ontology Construction (arxiv 2602.01276) | Paper | LLMs pueden construir ontologías automáticamente desde texto |
| Knowledge Graph 2025 — Semantic Arts | Industria | KGs + ontologías en inflection point de adopción enterprise (Gartner Slope of Enlightenment) |
| LightRAG (Oct 2024) — 10x token reduction vs GraphRAG | Tool | KG-based retrieval viable en producción |
| BPROP_001 technical notes | Propuesta interna | Ontología versionada + HITL injection |

### Hallazgo — LLMs pueden construir ontologías automáticamente
La investigación 2024-2026 muestra que los LLMs pueden construir grafos de conocimiento y ontologías automáticamente desde texto (arxiv 2602.01276, 2604.20795). Esto valida la viabilidad técnica de BPROP_001: los agentes del framework pueden generar entradas ontológicas desde los documentos que producen (BDRs, TDRs, etc.) sin que sea un paso manual separado.

### Entrada ontológica
```yaml
concept: "llm-automated-ontology-construction-versioned-evidence"
captured_date: "2026-06-14"
sources:
  - {id: "llm-ontology-construction-2026", type: "paper", title: "LLM-Driven Ontology Construction for Enterprise Knowledge Graphs", url: "https://arxiv.org/abs/2602.01276", year: 2026}
  - {id: "kg-2025-semanticarts", type: "industry", title: "Knowledge Graph 2025 — Gartner Slope of Enlightenment", year: 2025}
  - {id: "lightrag-2024", type: "tool", title: "LightRAG — 10x token reduction vs GraphRAG", year: 2024}
informs_artifacts: ["BPROP_001"]
confidence: MEDIUM
stale_after_months: 12
injected_by: "agent"
finding: "VALIDATES BPROP_001 — LLMs can auto-construct ontology entries from governance artifacts. Reduces HITL manual burden."
```

---

## RESUMEN EJECUTIVO — Nivel Architecture

### Scorecard de decisiones

| Decisión | ADR | Veredicto | Acción |
|:---------|:----|:----------|:-------|
| Adapter Pattern | ADR_001 | ✅ CONFIRMADO | Future Horizon: evaluar MCP en v2.0+ |
| Git backbone + ADRs | ADR_002 | ✅ CONFIRMADO | Fitness functions en GIR_001 validadas independientemente |
| Context distillation 9,500t | ADR_003 | ✅ CONFIRMADO | Observación: evaluar LLMLingua v2.0 |
| YAML State Machine | ADR_004 | ✅ CONFIRMADO | Temporal para v3.0 Enterprise (Future Horizon) |
| Orchestration single-agent | ADR_003 | ✅ CONFIRMADO | EU AI Act Article 14 compliance = diferenciador |
| Project Registry | ADR_005 | ✅ CONFIRMADO | — |
| Brownfield Onboarding | ADR_006 | ✅ CONFIRMADO | — |

### Propuestas de cambio (no invalidaciones, sino mejoras)

1. **PROPUESTA INMEDIATA (v1.0):** Agregar DeepEval (MIT, pytest-native) como dependencia de testing. Nuevo TDR_028 extensión de TREQ_039. Reemplaza revisión manual en integration TEST_TICKETs de comportamiento de agentes. Costo: bajo (compatible con pytest existente).

2. **FUTURE HORIZON v2.0:** Evaluar migración parcial a MCP para herramientas adicionales más allá de Git+Filesystem. Evaluar LLMLingua para context compression más eficiente. Evaluar Braintrust para eval collaboration cuando haya team.

3. **FUTURE HORIZON v3.0:** Temporal para workflow durability en deployments enterprise multi-proyecto concurrentes.

4. **BPROP_001 — VALIDADO:** La ontología versionada es técnicamente viable con LLMs construyendo entradas automáticamente desde artefactos de gobernanza. El mercado (Knowledge Graphs en Gartner Slope of Enlightenment) confirma el timing.

### Diferenciador competitivo identificado
**EU AI Act Article 14 compliance** — Nuestro modelo HITL (cooling-off transitions + sign-off explícito + Ledger de eventos) cumple los tres requisitos del Acto: context, authority, rationale en cada decisión. Ningún framework de SDLC que encontramos hace esto explícitamente. Es un ángulo de posicionamiento real para el mercado enterprise europeo y regulado.

