# Índice de Ontología — SDLC Governance Framework v1.0.0
## Estado del proceso adversarial (TREQ_045)
**Fecha:** 2026-06-14

---

## Resumen de Verdicts

| ID | Concepto | Capa | Verdict | Estado |
|:---|:---------|:-----|:--------|:-------|
| ONT_001 | Adapter Pattern (ADR_001) | Architecture | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_002 | Git Backbone + ADRs (ADR_002) | Architecture | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_003 | Context Distillation 9,500t (ADR_003) | Architecture | MATIZADA | DRAFT |
| ONT_004 | YAML State Machine (ADR_004) | Architecture | CONFIRMADA v1.0 / REQUIERE_REVISIÓN v2.0+ | DRAFT |
| ONT_005 | Single-Agent HITL Orchestration | Architecture | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_006 | BMAD — Competitor Reference | Business | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_007 | Human-Centric AI2L Model | Business | CONFIRMADA_CON_RIESGO | DRAFT |
| ONT_008 | Multi-Layer Specialized Agents | Business | CONFIRMADA | DRAFT |
| ONT_009 | Market Timing 2026 | Business | CONFIRMADA (Enterprise es el pagador) | DRAFT |
| ONT_010 | Spec-Driven Development Paradigm | Product | CONFIRMADA | DRAFT |
| ONT_011 | CLI/Chat Interaction Model | Product | CONFIRMADA v1.0 (técnico) | DRAFT |
| ONT_012 | Atomic File Writes | Technical | CONFIRMADA v1.0 | DRAFT |
| ONT_013 | Artifact Dependency Graph | Technical | CONFIRMADA_CON_RIESGO (arc-kit) | DRAFT |
| ONT_014 | DeepEval pytest plugin | Technical | DIFERIDO (ver nota) | DEFERRED |
| ONT_015 | LLM Ontology Construction | Technical | CONFIRMADA con alcance preciso | DRAFT |
| ONT_016 | Eval Harnesses Comparison | Technical | DIFERIDO (ver nota) | DEFERRED |

---

## Cambios respecto a la investigación confirmatoria original

| ID | Verdict original | Verdict adversarial | Delta |
|:---|:----------------|:--------------------|:------|
| ONT_001 | CONFIRMS | CONFIRMADA_CON_RIESGO | MCP security risks documentados |
| ONT_002 | CONFIRMS | CONFIRMADA_CON_RIESGO | arc-kit existe — estudiar antes de construir Librarian |
| ONT_003 | CONFIRMS | MATIZADA | Anthropic compact API puede hacer ceiling fijo innecesario |
| ONT_004 | CONFIRMS | CONFIRMADA v1.0 / REQUIERE_REVISIÓN v2.0+ | Durabilidad en OS crash es riesgo real |
| ONT_005 | CONFIRMS | CONFIRMADA_CON_RIESGO | Paralelismo selectivo dentro de capa es oportunidad |
| ONT_006 | CONFIRMS | CONFIRMADA_CON_RIESGO | BMAD construyendo plataforma web — ventana corta |
| ONT_007 | CONFIRMS | CONFIRMADA_CON_RIESGO | UX del HITL crítica — HITL mal diseñado es peor que no tener |
| ONT_008 | CONFIRMS | CONFIRMADA | PROFILE_LITE ya mitiga el riesgo de sobre-rigidez |
| ONT_009 | CONFIRMS | CONFIRMADA (precisado) | Enterprise paga, Developer Solo adopta gratis |
| ONT_010 | CONFIRMS | CONFIRMADA | PROFILE_LITE mitiga compresión de fases del mercado |
| ONT_011 | CONFIRMS | CONFIRMADA v1.0 | MCP server como integration path para IDEs — nueva oportunidad |
| ONT_012 | CONFIRMS | CONFIRMADA v1.0 | os.replace() + dir=target.parent es el detalle correcto |
| ONT_013 | CONFIRMS | CONFIRMADA_CON_RIESGO | arc-kit — mismo hallazgo que ONT_002 |
| ONT_015 | VALIDATES | CONFIRMADA con alcance | LLMs generan drafts, HITL aprueba — no automatización completa |

---

## Hallazgos transversales (aparecen en múltiples entradas)

### 1. arc-kit (github.com/tractorjuice/arc-kit)
Aparece en ONT_002 y ONT_013. Enterprise Architecture Governance
Harness que implementa ADRs, dependency tracking, roadmaps, risk registers.
**Acción pendiente:** estudiar arc-kit antes de implementar el
Librarian completo — puede tener overlap significativo.

### 2. MCP Security
Aparece en ONT_001. El mismo protocolo que habilita interoperabilidad
estandariza el attack surface. Typosquatting, tool poisoning, command
injection son riesgos documentados (arxiv 2604.05969).
**Acción pendiente:** antes de adoptar MCP en v2.0, evaluar security
posture. Relacionado con la nota estratégica de seguridad en BOOTSTRAPPING_NOTES.

### 3. Anthropic compact-2026-01-12 API
Aparece en ONT_003. Automatiza compresión de contexto (68% reducción,
91% retención). Puede hacer el ceiling fijo de 9,500 tokens parcialmente
redundante. **Acción pendiente:** evaluar compact API vs ceiling fijo.
Impacta TDR_003 — potencialmente MATIZADA.

### 4. Paralelismo selectivo dentro de capas
Aparece en ONT_005. Agentes dentro de la misma capa pueden correr en
paralelo con un solo HITL sign-off en batch. Reduce tiempo de planeación
sin comprometer governance entre capas.
**Acción pendiente:** proponer como mejora en PDR_001 roadmap — posible
v1.1.0 o v2.0 feature.

### 5. MCP server del Librarian
Aparece en ONT_011. En lugar de construir GUI propia, exponer el
Librarian como MCP server permite que IDEs con soporte MCP (Kiro,
Cursor, Claude Code) consuman el framework sin interfaz adicional.
**Acción pendiente:** evaluar como alternativa a la plataforma web
(BDR_022) para adopción temprana más rápida.

---

## ONT_014 y ONT_016 — Estado de deferimiento
Ambas entradas cubren herramientas de evaluación de agentes (DeepEval
vs Inspect AI). Diferidas per decisión HITL para ser revisadas juntas
en la ronda de Architecture cuando se tome la decisión técnica sobre
qué herramienta(s) usar y en qué milestone entran.

---

## Pendiente: Sign-off HITL
Las 14 entradas en estado DRAFT requieren revisión y sign-off del HITL
per BREQ_023 y TREQ_045 antes de ser inyectables en el contexto
de los agentes en runtime.

Proceso: HITL revisa cada digest, confirma que la síntesis con tensión
es honesta y que el verdict está calibrado con la evidencia, y firma.
Entradas con DEFERRED no requieren sign-off — se reabren cuando llegue
su turno.
