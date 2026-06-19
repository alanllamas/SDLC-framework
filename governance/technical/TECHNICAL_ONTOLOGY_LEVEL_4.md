# Ontología de Validación — Nivel Technical
## SDLC Governance Framework v1.0.0
**Fecha de captura:** 2026-06-14
**Auditor:** Tech Lead / Evidence Collection Pass

---

## DECISIÓN 1 — TDR_007 (atomic writes) / TREQ_007: temp-then-rename pattern

### Lo que decidimos
Todas las escrituras del Librarian usan un patrón temp-then-rename para garantizar atomicidad.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| POSIX estándar — `os.rename()` garantizado atómico en mismo filesystem | Estándar | ✅ CONFIRMA: el patrón es correcto |
| bswen.com 2026 — "atomic file writing Python, temp in same directory" | Industria | ✅ CONFIRMA: temp en mismo directorio es crítico |
| ActiveState — "rename is guaranteed atomic on POSIX Linux/Unix" | Comunidad | ✅ CONFIRMA |
| SQLite/PostgreSQL — WAL pattern análogo | Precedente | ✅ CONFIRMA: mismo principio en DBs de producción |
| `python-atomicwrites` (deprecated) → `os.replace` (Python 3.3+) | Library | ✅ `os.replace` es el estándar actual |

### Hallazgo — `os.replace` vs `os.rename`
La biblioteca `python-atomicwrites` fue deprecada porque Python 3.3+ provee `os.replace()` que maneja el caso de Windows correctamente (a diferencia de `os.rename()` que no es atómico en Windows para overwrites). Nuestra especificación debería usar `os.replace()` explícitamente en lugar del genérico "rename" para claridad y portabilidad.

**Riesgo identificado:** si temp file y target están en diferentes filesystems, `os.replace()` ya no es atómico (cae back a copy+delete). La spec debe asegurar que NamedTemporaryFile se crea con `dir=target.parent` para garantizar mismo filesystem.

### Veredicto
**✅ CONFIRMA TDR_007/TREQ_007** con una precisión técnica: usar `os.replace()` explícitamente y `dir=target.parent` en el NamedTemporaryFile. Esto es un refinamiento de implementación, no un cambio de decisión.

### Entrada ontológica
```yaml
concept: "atomic-file-writes-posix-temp-rename-python"
captured_date: "2026-06-14"
sources:
  - {id: "posix-rename-atomic", type: "standard", title: "POSIX os.rename() atomicity guarantee on same filesystem"}
  - {id: "python-os-replace", type: "stdlib", title: "Python 3.3+ os.replace() — atomic on POSIX and Windows for overwrite", url: "https://docs.python.org/3/library/os.html#os.replace"}
  - {id: "atomicwrites-deprecated", type: "library", title: "python-atomicwrites deprecated — os.replace() now preferred", url: "https://github.com/untitaker/python-atomicwrites"}
  - {id: "sqlite-wal-pattern", type: "precedent", title: "SQLite WAL: write to temp log file, then checkpoint to main DB — same principle"}
informs_artifacts: ["TDR_007", "TREQ_007", "TREQ_028"]
confidence: HIGH
stale_after_months: 36
injected_by: "agent"
finding: "CONFIRMS with implementation refinement: use os.replace() explicitly, NamedTemporaryFile(dir=target.parent) to guarantee same-filesystem atomicity. Windows-safe."
```

---

## DECISIÓN 2 — TDR_011: Dependency Graph para traceability (build_dependency_graph)

### Lo que decidimos
Usar un grafo de dependencias en memoria construido desde los `relationships` de cada artefacto para impact analysis, GIR planes 1-3, y generación de diagramas.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| SoK: Systematizing Software Artifacts Traceability (arxiv 2603.16208) | Paper | ✅ CONFIRMA: artifact traceability graphs son práctica de investigación activa |
| DHS S&T — "Software Artifact Dependency Graph (ADG) Generation" solicitation 2024 | Gobierno | ✅ CONFIRMA: ADGs son un problema real buscando soluciones a nivel gobierno |
| "Who's Who? LLM-assisted Software Traceability" (arxiv 2511.02434) | Paper | ✅ LLMs usados para traceability link recovery — valida nuestro approach |
| RSTrace+ — Reviewer suggestion via traceability graphs | Paper | ✅ Traceability graphs para análisis secundario (nosotros: impact analysis) |

### Hallazgo — ADG como área de interés gubernamental
El DHS (Dept. of Homeland Security) de EE.UU. lanzó una solicitud en 2024 buscando capacidades de "Software Artifact Dependency Graph Generation" para entender, gestionar y reducir riesgo en software de infraestructura. Esto valida que los dependency graphs como mecanismo de governance son una preocupación de nivel estratégico, no solo una elegancia técnica.

### Hallazgo — LLMs para traceability link recovery
El paper "Who's Who? LLM-assisted Software Traceability" usa LLMs para establecer trace links entre artefactos cuando estos no están documentados explícitamente. Esto es interesante para BPROP_001 (ontología): si un proyecto tiene artefactos sin `relationships` bien definidas, un agente podría inferirlas — potencialmente útil para Brownfield Onboarding (v0.13.0 Path D).

### Veredicto
**✅ CONFIRMA TDR_011.** Dependency graphs para artifact traceability son práctica de investigación y gobierno validada. El approach de construir el grafo en memoria desde front-matter es correcto para nuestro scope.

### Entrada ontológica
```yaml
concept: "software-artifact-dependency-traceability-graph"
captured_date: "2026-06-14"
sources:
  - {id: "sok-artifacts-traceability-2026", type: "paper", title: "SoK: Systematizing Software Artifacts Traceability via Associations, Techniques, and Applications", url: "https://arxiv.org/abs/2603.16208", year: 2026}
  - {id: "dhs-adg-2024", type: "government", title: "DHS S&T: Software Artifact Dependency Graph Generation solicitation", url: "https://www.dhs.gov/science-and-technology/news/2024/08/16/st-seeks-solutions-software-artifact-dependency-graph-generation", year: 2024}
  - {id: "llm-traceability-2025", type: "paper", title: "LLM-assisted Software Traceability with Architecture Entity Recognition", url: "https://arxiv.org/abs/2511.02434", year: 2025}
informs_artifacts: ["TDR_011", "TDR_015", "TDR_024", "TREQ_022"]
confidence: HIGH
stale_after_months: 18
injected_by: "agent"
finding: "CONFIRMS. DHS-level interest in ADGs validates governance use case. LLM-assisted traceability link recovery interesting for v0.13.0 brownfield onboarding."
```

---

## DECISIÓN 3 — TDR_001 (Python 3.11+): Elección del lenguaje

### Lo que decidimos
Python 3.11+ como único lenguaje del framework.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| Stack Overflow Dev Survey 2025 — "84% of developers use AI tools" | Benchmark | Contexto, no lenguaje específico |
| Menlo Ventures 2025 — "AI code generation: $4B enterprise spend, 4.1x YoY" | Market | Contexto |
| GitHub Copilot — usado con todos los lenguajes | Producto | Neutral |
| Frameworks competidores: BMAD (Node.js + Python), GSD (Node.js), Spec Kit (Python) | Competidores | ⚠️ BMAD requiere Node.js 20.12+ Y Python 3.10+ |

### Hallazgo — BMAD v6 requiere AMBOS Node.js y Python
BMAD v6 requiere Node.js 20.12+ y uv package manager además de Python — lo que añade complejidad de setup. Nuestro Python-only approach es más simple para el Developer Solo profile. Sin embargo, dado que el ecosistema de AI tooling (LangChain, LangGraph, DeepEval) es Python-first con algunas excepciones Node.js, Python-only es la decisión correcta.

### Veredicto
**✅ CONFIRMA TDR_001.** Python-only es correcto y más simple que la alternativa BMAD. El ecosistema de AI tooling converge en Python.

---

## DECISIÓN 4 — TDR_015 (GIR Engine): Five-Plane Verification como graph traversals

### Lo que decidimos
Los cinco planos de verificación son traversals sobre el dependency graph de TDR_011. No nueva infraestructura.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| elevateconsult 2026 — "Evidence maps: dynamic relationships linking requirements to artifacts" | Industria | ✅ CONFIRMA: nuestros 5 planos son "evidence maps" automatizados |
| EU AI Act / NIST AI RMF / ISO 42001 — audit trail requirements | Regulación | ✅ CONFIRMA: compliance requiere exactamente lo que GIR_001 produce |
| BMAD Issue #2003 — "lacks quality controls to prevent dev agents bypassing controls" | Gap competidor | ✅ CONFIRMA que GIR es diferenciador real |
| DHS ADG solicitation 2024 — "understand, manage, and reduce risk to software" | Gobierno | ✅ CONFIRMA: risk management via dependency graph es problema real |

### Veredicto
**✅ CONFIRMA TDR_015.** Five-plane verification via dependency graph traversals es el approach correcto. Evidence maps = nuestro GIR en terminología de la industria.

---

## DECISIÓN 5 — TDR_003 (Context Distillation Engine): 9,500-token ceiling

### Evidencia encontrada (del nivel Architecture, reitero en Technical para completitud)
| Fuente clave | Veredicto |
|:-------------|:----------|
| MatClaw 2025 — "caps effective context at 200K for 1M models" | ✅ Conservative caps son práctica correcta |
| "Complexity Trap" 2025 — "observation masking as efficient as LLM summarization" | ⚠️ Observación: destilación LLM puede ser costosa |
| ContextBudget 2026 — budget-aware compression | ✅ Budget-aware approach es investigación activa |

### Hallazgo técnico adicional
El paper de ContextBudget (arxiv 2604.01664) señala dos failure modes en context management: sobre-compresión (borra evidencia crítica) y sub-compresión (overflow del contexto). Nuestro 9,500-token ceiling fijo cae en la trampa de ser "budget-free" — no se adapta dinámicamente a la complejidad de la sesión. Para sesiones simples, 9,500 es mucho. Para sesiones complejas con muchos artefactos activos, podría ser insuficiente.

**Propuesta de refinamiento:** En v2.0, considerar un ceiling dinámico: `min(9,500, available_context * 0.15)` — usando 15% del contexto disponible, ajustado por el modelo en uso.

### Veredicto
**✅ CONFIRMA TDR_003 para v1.0** con observación para v2.0: evaluar ceiling dinámico en lugar de fijo.

---

## DECISIÓN 6 — TDR_022 (PytestRunnerAdapter): pytest como test runner

### Lo que decidimos
`subprocess pytest` como implementación del TestRunnerAdapter, sin dependencias adicionales más allá de pytest.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| DeepEval v4.0.3 — "pytest-native, MIT license" | Tool | ⚠️ NUEVA: DeepEval se integra COMO pytest plugin |
| Inspect AI v0.3.225 — UK AI Security Institute | Tool | ⚠️ NUEVA: gobierno respaldado, estándar emergente |
| "Solo ML engineer: DeepEval (MIT, runs locally, no infrastructure)" | Análisis | ✅ Confirma DeepEval para Developer Solo profile |

### Hallazgo crítico — DeepEval como pytest plugin
DeepEval no es una herramienta separada que reemplaza pytest — **es un plugin de pytest**. Los tests de evaluación de agentes se escriben exactamente igual que tests pytest estándar, con métricas adicionales. Esto significa que `PytestRunnerAdapter` (TREQ_039) puede ejecutar TANTO unit tests tradicionales COMO evaluaciones de agentes DeepEval en el mismo `pytest` call. No se necesita un nuevo adapter — el mismo `subprocess pytest` ejecuta ambos tipos de tests si los test files incluyen evaluaciones DeepEval.

```python
# Ejemplo de test DeepEval que corre via pytest normal:
from deepeval import evaluate
from deepeval.metrics import GEval
from deepeval.test_case import LLMTestCase

def test_chronicler_summarizes_faithfully():
    test_case = LLMTestCase(
        input="Diff for DEV_001",
        actual_output=chronicler.generate_summary(diff),
        expected_output=None  # no golden answer needed
    )
    metric = GEval(
        name="Faithfulness",
        criteria="Summary accurately reflects the diff without hallucination"
    )
    metric.measure(test_case)
    assert metric.score >= 0.8
```

Esto resuelve el "cómo integramos AI harnesses" sin cambiar TREQ_039 ni necesitar un nuevo TDR. Solo se añade DeepEval como dependencia de testing.

### Veredicto
**✅ CONFIRMA TDR_022** y añade una propuesta concreta: DeepEval como pytest plugin se integra sin cambios al PytestRunnerAdapter. Las evaluaciones de agentes coexisten con unit tests en el mismo `pytest` run.

### Entrada ontológica
```yaml
concept: "deepeval-pytest-plugin-agent-behavioral-testing"
captured_date: "2026-06-14"
sources:
  - {id: "deepeval-v4-pytest", type: "tool", title: "DeepEval v4.0.3 — pytest-native plugin for LLM evaluation, MIT license", url: "https://github.com/confident-ai/deepeval"}
  - {id: "inspect-ai-ukaisc", type: "tool", title: "Inspect AI v0.3.225 — UK AI Security Institute open source eval framework", url: "https://github.com/UKGovernmentBEIS/inspect_ai"}
  - {id: "inference-net-deepeval-solo", type: "analysis", title: "Solo ML engineer: DeepEval (MIT, local, no infrastructure) is recommended", url: "https://inference.net/content/llm-evaluation-tools-comparison/", year: 2026}
informs_artifacts: ["TDR_022", "TREQ_039", "TREQ_040", "BPROP_001"]
confidence: HIGH
stale_after_months: 6
injected_by: "agent"
finding: >
  DeepEval is a pytest PLUGIN — not a separate tool. PytestRunnerAdapter (TREQ_039)
  runs DeepEval evaluations automatically via normal pytest subprocess call.
  No new TDR needed. Just add DeepEval as test dependency.
  This closes the AI harness gap (BPROP_001) for v1.0 with minimal friction.
```

---

## DECISIÓN 7 — TDR_024 (Diagram Generation Engine): Mermaid como formato de salida

### Lo que decidimos
Mermaid `.mmd` como formato de diagrama, generado desde el dependency graph de TDR_011.

### Evidencia encontrada
| Fuente | Tipo | Veredicto |
|:-------|:-----|:----------|
| GitHub nativo soporta Mermaid en markdown | Producto | ✅ CONFIRMA: render gratuito en el repo |
| BMAD v6 — genera "architecture diagrams" | Competidor | Mencionado pero formato no especificado |
| Structurizr (C4 model) — alternativa de diagramación | Tool | Considerado, más pesado |

### Hallazgo — Mermaid es el estándar de facto 2025-2026
GitHub, GitLab, Notion, Obsidian, y casi todos los markdown viewers soportan Mermaid nativo en 2026. Es plain text, git-diffable, y no requiere herramientas adicionales para visualizar. La decisión de Mermaid sobre alternativas (Graphviz, PlantUML, Structurizr) es correcta para nuestro perfil de Developer Solo con zero-additional-infrastructure.

### Veredicto
**✅ CONFIRMA TDR_024.** Mermaid es el estándar de facto para diagramas en markdown/git.

---

## RESUMEN EJECUTIVO — Nivel Technical

### Scorecard de decisiones Technical

| Decisión | TDR | Veredicto | Observación |
|:---------|:----|:----------|:------------|
| Atomic writes (temp-then-rename) | TDR_007 | ✅ CONFIRMADO | Usar `os.replace()` explícitamente, `dir=target.parent` |
| Dependency Graph para traceability | TDR_011 | ✅ CONFIRMADO | DHS-level interest valida governance use case |
| Python 3.11+ single language | TDR_001 | ✅ CONFIRMADO | Más simple que BMAD (Node.js + Python) |
| GIR five-plane graph traversals | TDR_015 | ✅ CONFIRMADO | "Evidence maps" en terminología de industria |
| Context 9,500-token ceiling | TDR_003 | ✅ CONFIRMADO (v1.0) | v2.0: evaluar ceiling dinámico |
| pytest subprocess TestRunnerAdapter | TDR_022 | ✅ CONFIRMADO + MEJORA | DeepEval = pytest plugin, corre en mismo subprocess |
| Mermaid diagram generation | TDR_024 | ✅ CONFIRMADO | Estándar de facto 2026 |

### Hallazgo más importante: DeepEval como pytest plugin
Este es el hallazgo técnico de mayor impacto inmediato: **DeepEval no requiere cambios a TREQ_039**. Es un plugin de pytest que corre en el mismo subprocess. Añadir DeepEval como dependencia de testing cierra el gap de AI evaluation harnesses (BPROP_001) para v1.0 sin nuevo TDR, sin nuevo adapter, sin nueva infraestructura. Es la solución más barata y directa identificada en toda esta investigación.

### Refinamiento técnico identificado
`os.replace()` (Python 3.3+) con `dir=target.parent` en NamedTemporaryFile es la implementación correcta y portable de atomic writes. Debe ser especificado explícitamente en TREQ_007 durante la implementación (DEV_043 o equivalente).

