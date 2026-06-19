---
id: ONT_013
concept: "software-artifact-dependency-traceability-graph"
layer: "technical"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "HIGH"
stale_after_months: 18
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_013_original"
    link_reason: "Revisión adversarial con hallazgo nuevo: arc-kit."
---

# ONT_013 — Software Artifact Dependency Traceability Graph
## Digest Adversarial Verificado — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
Esta decisión estaría equivocada si el grafo de dependencias
construido en memoria es demasiado lento o frágil para el corpus
de artefactos real del framework (~250+ artefactos).

---

## FASE 1 — Evidencia

### A — Evidencia a Favor
DHS S&T 2024: busca capacidades de "Software Artifact Dependency
Graph Generation" activamente. Valida que el problema es real y
la solución tiene demanda gubernamental.
LLM-assisted traceability (arxiv 2511.02434): LLMs pueden inferir
trace links faltantes — útil para brownfield onboarding (v0.13.0).

### B — Evidencia en Contra
arc-kit (tractorjuice/arc-kit — github): Enterprise Architecture
Governance Harness que ya implementa dependency tracking entre
artefactos con 72 comandos. El Librarian que construimos puede
estar reinventando partes de arc-kit.

### C — Alternativa Lateral
Adoptar arc-kit como Librarian base en lugar de construirlo desde cero.
Evaluar compatibilidad antes de construir el Librarian completo.

---

## FASE 2 — Síntesis
El grafo en memoria es correcto para GIR_001 (auditorías infrequentes).
Riesgo: arc-kit existe con más features. Acción antes de construir:
estudiar arc-kit para identificar overlap y decidir adoptar vs construir.

**Veredicto:** CONFIRMADA_CON_RIESGO
Riesgo de reinvención: estudiar arc-kit antes de implementar el Librarian.

**Fuentes:** DHS ADG 2024, arxiv 2511.02434, github.com/tractorjuice/arc-kit.
