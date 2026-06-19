# Índice de Ontología — SDLC Governance Framework v1.0.0
## Proceso adversarial TREQ_045 — Estado completo
**Fecha de cierre:** 2026-06-18

---

## Tabla de Verdicts

| ID | Concepto | Capa | Verdict | Estado |
|:---|:---------|:-----|:--------|:-------|
| ONT_001 | Adapter Pattern (ADR_001) | Architecture | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_002 | Git Backbone + ADRs (ADR_002) | Architecture | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_003 | Context Distillation 9,500t | Architecture | MATIZADA | DRAFT |
| ONT_004 | YAML State Machine (ADR_004) | Architecture | CONFIRMADA v1.0 / REQUIERE_REVISIÓN v2.0+ | DRAFT |
| ONT_005 | Single-Agent HITL Orchestration | Architecture | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_006 | BMAD — Competitor Reference | Business | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_007 | Human-Centric AI2L Model | Business | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_008 | Multi-Layer Specialized Agents | Business | CONFIRMADA | DRAFT |
| ONT_009 | Market Timing 2026 | Business | CONFIRMADA (Enterprise paga) | DRAFT |
| ONT_010 | Spec-Driven Development | Product | CONFIRMADA | DRAFT |
| ONT_011 | CLI/Chat Interaction Model | Product | CONFIRMADA v1.0 + OPORTUNIDAD | DRAFT |
| ONT_012 | Atomic File Writes | Technical | CONFIRMADA v1.0 | DRAFT |
| ONT_013 | Artifact Dependency Graph | Technical | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_014 | DeepEval pytest plugin | Technical | DEFERRED | DEFERRED |
| ONT_015 | LLM Ontology Construction | Technical | CONFIRMADA alcance preciso | DRAFT |
| ONT_016 | Eval Harnesses Comparison | Technical | DEFERRED | DEFERRED |
| ONT_017 | Ponytail Decision Ladder | Technical | CONFIRMADA_CON_RIESGO | DRAFT |

---

## Delta vs investigación confirmatoria original

| ID | Original | Adversarial | Qué cambió |
|:---|:---------|:------------|:-----------|
| ONT_001 | CONFIRMS | CONFIRMADA_CON_RIESGO | MCP security risks — attack surface ampliado |
| ONT_002 | CONFIRMS | CONFIRMADA_CON_RIESGO | arc-kit existe — estudiar antes de construir Librarian |
| ONT_003 | CONFIRMS | MATIZADA | Anthropic compact API puede hacer ceiling fijo sub-óptimo |
| ONT_004 | CONFIRMS | CONFIRMADA v1.0 / REQUIERE_REVISIÓN v2.0+ | Durabilidad OS crash es riesgo real |
| ONT_005 | CONFIRMS | CONFIRMADA_CON_RIESGO | Paralelismo selectivo dentro de capa es oportunidad |
| ONT_006 | CONFIRMS | CONFIRMADA_CON_RIESGO | BMAD construyendo plataforma — ventana corta |
| ONT_007 | CONFIRMS | CONFIRMADA_CON_RIESGO | UX del HITL crítica — HITL mal diseñado es peor que no tener |
| ONT_008 | CONFIRMS | CONFIRMADA | PROFILE_LITE mitiga compresión de fases |
| ONT_009 | CONFIRMS | CONFIRMADA precisado | Enterprise paga, Developer Solo adopta gratis |
| ONT_010 | CONFIRMS | CONFIRMADA | PROFILE_LITE mitiga presión de velocidad |
| ONT_011 | CONFIRMS | CONFIRMADA + OPORTUNIDAD | MCP server del Librarian como integration path para IDEs |
| ONT_012 | CONFIRMS | CONFIRMADA v1.0 | os.replace() + dir=target.parent es el detalle correcto |
| ONT_013 | CONFIRMS | CONFIRMADA_CON_RIESGO | arc-kit — mismo hallazgo que ONT_002 |
| ONT_015 | VALIDATES | CONFIRMADA alcance | LLMs generan drafts, HITL aprueba |
| ONT_017 | NUEVA | CONFIRMADA_CON_RIESGO | Ponytail — patrón validado, benchmarks auto-reportados |

---

## Hallazgos transversales críticos

### 1. arc-kit (ONT_002 + ONT_013)
github.com/tractorjuice/arc-kit — Enterprise Architecture Governance
Harness con ADRs, dependency tracking, roadmaps, risk registers.
**Acción antes de construir Librarian:** estudiar overlap.

### 2. Anthropic compact API (ONT_003)
compact-2026-01-12: 68% reducción de tokens, 91% retención.
**Acción:** evaluar si hace el ceiling fijo de 9,500 sub-óptimo.
Impacta TDR_003.

### 3. MCP security risks (ONT_001)
Action tools: 27% → 65% de todos los MCP tools. Typosquatting,
tool poisoning documentados. **Acción:** evaluar security posture
antes de adoptar MCP en v2.0. Relacionado con nota estratégica
de seguridad en BOOTSTRAPPING_NOTES.

### 4. Paralelismo selectivo (ONT_005)
Agentes dentro de la misma capa pueden correr en paralelo con
batch sign-off. Reduce tiempo sin comprometer governance entre capas.
**Acción:** proponer como feature en v1.1.0 o v2.0.

### 5. MCP server del Librarian (ONT_011)
Integration path para IDEs con soporte MCP (Kiro, Cursor, Claude Code)
sin construir GUI propia. **Acción:** evaluar como alternativa más
rápida a plataforma web (BDR_022) para adopción temprana.

### 6. Ponytail decision ladder (ONT_017)
Principio validado: pre-evaluar antes de ejecutar reduce costo API
47-77% en código. **Acción:** incorporar decision ladder de governance
en OPERATIONAL_MANDATEs. Compatibilidad explícita con Ponytail
en Phase 2 como feature de adopción.

---

## Pendientes de sign-off HITL
15 entradas en DRAFT requieren revisión y firma del HITL per BREQ_023.
2 entradas DEFERRED (ONT_014, ONT_016) — se retoman en ronda
de Architecture cuando se decida sobre herramientas de evaluación.
