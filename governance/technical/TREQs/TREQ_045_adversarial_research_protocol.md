---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_045
type: TREQ
title: "Adversarial Research Protocol — Ontology Evidence Collection"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.13.0 — Epistemic Charter & Adversarial Process"
relationships:
  vertical:
    governing_bdr: BDR_018
    parent_id: BREQ_023
  horizontal:
    depends_on: [TREQ_044]
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-14T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# TREQ_045 — Adversarial Research Protocol

## 1. Problema que resuelve
La investigación confirmatoria produce ontología sesgada.
Buscar evidencia de que nuestras decisiones son correctas
garantiza encontrarla — porque el espacio de posibles
fuentes es suficientemente grande para confirmar casi
cualquier posición.

Este protocolo define el proceso de construcción de evidencia
que el framework usa para informar decisiones, con tres
direcciones de búsqueda obligatorias y una hipótesis de
falsificación explícita antes de buscar.

---

## 2. El Protocolo — Cuatro Fases por Decisión

### FASE 0 — Hipótesis de Falsificación (antes de buscar)
El agente formula ANTES de buscar cualquier evidencia:

```
DECISIÓN: [ID y título de la decisión]
HIPÓTESIS_DE_FALSIFICACIÓN: "Esta decisión estaría equivocada si..."
SEÑALES_DE_FALSIFICACIÓN: [lista de lo que buscaríamos que la refutaría]
CONDICIÓN_DE_CAMBIO: "Cambiaríamos esta decisión si encontráramos..."
```

El HITL revisa y complementa esta hipótesis antes de
que el agente arranque la búsqueda. Si el HITL no puede
articular ninguna condición de cambio, esa es señal de
que el sesgo de confirmación ya está activo.

---

### FASE 1 — Búsqueda en Tres Direcciones (simultáneas)

**Dirección A — Evidencia a favor:**
¿Qué papers, proyectos, benchmarks, herramientas, expertos
apoyan esta decisión?
- Búsqueda normal, criterio: fuentes independientes del
  framework y del agente que busca.

**Dirección B — Evidencia en contra:**
¿Qué papers, proyectos, benchmarks, herramientas, expertos
contradicen esta decisión o muestran que approaches similares
han fallado?
- Búsqueda activa de críticas, postmortems, papers de
  alternativas. Si no se encuentra nada en contra con
  esfuerzo real, eso se documenta explícitamente — no
  se omite.

**Dirección C — Alternativas laterales:**
¿Qué enfoques completamente distintos existen que resuelven
el mismo problema de una manera que no consideramos?
- No alternativas "paja" — alternativas serias que alguien
  podría preferir con razón legítima.

```python
class AdversarialResearchResult:
    decision_id: str
    falsification_hypothesis: str
    falsification_signals: list[str]
    evidence_for: list[EvidenceEntry]      # Dirección A
    evidence_against: list[EvidenceEntry]  # Dirección B
    lateral_alternatives: list[str]        # Dirección C
    falsification_found: bool  # ¿encontramos algo que la falsifica?
    falsification_evidence: list[EvidenceEntry] | None
```

**Regla de balance:** Dirección B y C deben tener al menos
el mismo número de fuentes que Dirección A. Si A tiene 4
fuentes y B tiene 1, la investigación no está completa.

---

### FASE 2 — Síntesis con Tensión

El digest de ontología NO es "evidencia confirma decisión X."
El digest es:

```markdown
## [ONT_XXX] — [Concepto]

**Decisión que informa:** [ID]

**Tensión central:**
[Resumen honesto de la tensión entre evidencia a favor
y evidencia en contra — en 2-3 oraciones]

**Evidencia a favor:**
- [fuente]: [hallazgo específico aplicable]
- [fuente]: [hallazgo específico aplicable]

**Evidencia en contra / riesgos:**
- [fuente]: [hallazgo específico que contradice o matiza]
- [fuente]: [condición bajo la cual la decisión falla]

**Alternativas laterales no tomadas:**
- [alternativa]: [por qué es válida y por qué no la elegimos]

**Hipótesis de falsificación:**
[La decisión estaría equivocada si encontráramos X — y
esto es lo que buscamos y encontramos/no encontramos]

**Veredicto:**
CONFIRMADA / CONFIRMADA_CON_RIESGO / MATIZADA / REQUIERE_REVISIÓN
[+ razón explícita]
```

Solo cuatro verdicts posibles — no "CONFIRMS" que parece
más positivo de lo que la evidencia justifica.

---

### FASE 3 — Sign-off con Criterios de Calidad

El HITL aprueba el digest SOLO si:

1. ✅ La Fase 0 (hipótesis de falsificación) fue completada
   antes de buscar evidencia.
2. ✅ Direcciones B y C tienen cobertura real (no token).
3. ✅ El digest refleja la tensión honestamente — si toda
   la evidencia "por casualidad" apunta en una dirección,
   el HITL debe cuestionar si la búsqueda fue realmente
   adversarial.
4. ✅ El verdict está calibrado con la evidencia — no
   es automáticamente CONFIRMADA.

Si alguno de estos no se cumple, el digest vuelve a DRAFT.

---

## 3. Aplicación Retroactiva — Ontología Existente

Las 16 entradas ONT_001-016 fueron generadas con búsqueda
confirmatoria. Deben ser revisadas con este protocolo:

1. Para cada entrada, formular la hipótesis de falsificación
   retroactivamente con el HITL.
2. Correr Direcciones B y C que no se corrieron originalmente.
3. Actualizar los digests con el formato de Síntesis con Tensión.
4. Re-validar verdicts — algunos CONFIRMS podrían cambiar
   a CONFIRMADA_CON_RIESGO o MATIZADA.

Prioridad de revisión: las entradas con stale_after_months
más corto (6 meses) son las más críticas porque son
también las más volátiles. ONT_006 (BMAD), ONT_009
(market timing), ONT_014 (DeepEval) — arrancar por estas.

---

## 4. Integración con Modelo de Costo (Hybrid LLM)

La investigación adversarial tiene un perfil de costo
distinto a otras tareas del framework:

- **Fase 0** (hipótesis): razonamiento puro → modelo local
  (Ollama) viable, no requiere calidad de producción.
- **Fase 1** (búsqueda): web search → no requiere LLM
  de producción, cualquier modelo puede sintetizar
  resultados de búsqueda.
- **Fase 2** (síntesis con tensión): requiere juicio
  epistémico real → Claude API.
- **Fase 3** (sign-off): HITL, no LLM.

Esto significa que solo Fase 2 consume tokens de producción
— el resto puede correr localmente o con búsqueda web
directa. El costo por decisión adversarialmente investigada
es aproximadamente el de una síntesis de ~2,000 tokens de
output.

---

## 5. Criterios de Verificación

- 100% de entradas de ontología tienen Fase 0 completada
  antes de la búsqueda.
- 0% de digests con Dirección B o C vacías o token.
- Ratio B+C/A >= 1.0 en número de fuentes.
- 0% de verdicts CONFIRMADA sin evidencia en contra
  que haya sido buscada y no encontrada (registrar
  "búsqueda en contra realizada, sin hallazgos" es válido
  — no buscar no lo es).
