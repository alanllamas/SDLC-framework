---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TDR_026
type: TDR
title: "Ontological Research Process — End-to-End Pipeline Specification"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.14.0 — Epistemic Charter & Cost Visibility"
relationships:
  vertical:
    governing_bdr: BDR_018
    parent_id: AREQ_002
  horizontal:
    depends_on: [TREQ_045, BREQ_023, BREQ_025, BPROP_001]
  cross_plane:
    architecture_adr: ADR_004
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-18T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# TDR_026 — Ontological Research Process
## End-to-End Pipeline Specification

---

## 1. Decision Context

* **Parent AREQ:** AREQ_002 — Adapter Implementation Pattern
* **Governing BDR:** BDR_018 — Governance Integrity Audit Mandate
* **Decision Scope:** Este TDR formaliza el proceso completo de
  investigación ontológica que el framework ejecutó iterativamente
  durante el ciclo de validación de v1.0 (sesiones 2026-06-14 a
  2026-06-18) pero que nunca existió como un artefacto único de
  gobernanza. Amarra TREQ_045, BREQ_023, BREQ_025, y BPROP_001 en
  un pipeline reproducible.

## 2. El Proceso — Seis Fases

### FASE 0.5 — Legal Context Scan (BREQ_025)
Trigger: distribution_regions o industry_vertical configurados,
o la decisión involucra datos/logs/modelos cloud/retención.
Output: LegalContextScan payload. Si
`professional_legal_review_required=true`, notificación visible
al HITL antes de continuar.

### FASE 0 — Hipótesis de Falsificación (TREQ_045)
Formulada ANTES de buscar evidencia. Tres elementos obligatorios:
`falsification_hypothesis`, `falsification_signals`,
`condición_de_cambio`. El HITL revisa y complementa antes de
que arranque la búsqueda.

### FASE 1 — Búsqueda en Tres Direcciones (TREQ_045)
Direcciones A (a favor), B (en contra), C (alternativas laterales)
— simultáneas, no secuenciales. Regla de balance: B+C debe tener
al menos tantas fuentes como A. Cada fuente web pasa por archivado
automático en Wayback Machine (`/ontology add` extension).

### FASE 2 — Síntesis con Tensión (TREQ_045)
El digest no es "evidencia confirma X" — es tensión explícita entre
A y B, alternativas C no tomadas, y un veredicto de cuatro valores
posibles: CONFIRMADA / CONFIRMADA_CON_RIESGO / MATIZADA /
REQUIERE_REVISIÓN. Nunca "CONFIRMS" sin calificación.

### FASE 3 — Sign-off HITL (TREQ_045 + BREQ_023)
HITL revisa: (1) Fase 0 completada antes de buscar, (2) B y C
con cobertura real, (3) tensión reflejada honestamente, (4) verdict
calibrado con evidencia — no automáticamente positivo. Solo entonces
`status: DRAFT → AUTHORIZED`.

### FASE 4 — Inyección en Runtime (BPROP_001 + AREQ_003)
Solo entradas `AUTHORIZED` son inyectables en el contexto de
agentes. El Orchestrator selecciona digests relevantes por
`informs_artifacts` match con el artefacto que el agente activo
está construyendo — Opción A de BPROP_001 (digest único, scope
general, sin granularidad por contexto de aplicación).

### FASE 5 — Staleness Check (BPROP_001)
Cada entrada tiene `stale_after_months`. Al vencer, el digest
vuelve a `DRAFT` automáticamente y el ciclo se reinicia desde
Fase 0.5 para esa entrada específica — no desde cero, sino
re-verificando si la evidencia capturada sigue vigente.

## 3. Estructura de Tres Capas (formalizada de BPROP_001)

```
governance/ontology/[layer]/
├── ONT_NNN_metadata.yaml      ← Capa 1: índice, fuentes, confidence
├── ONT_NNN_digest.md          ← Capa 2: 150-300 palabras, inyectable
└── ONT_NNN_raw/                ← Capa 3: fragmentos, URLs archivados
    ├── source_001_excerpt.md
    └── legal_context_scan.yaml
```

**Capa 1 (metadata):** nunca se inyecta en contexto de agente —
es índice para búsqueda, audit, y HITL.
**Capa 2 (digest):** la única capa inyectable. Generada en Fase 2,
aprobada en Fase 3.
**Capa 3 (raw):** HITL-driven, opcional, contiene evidencia
verificable que respalda el digest — fragmentos exactos, no
artículos completos (per copyright fair-use, BREQ_025 sección 6).

## 4. Reproducibilidad y Determinismo (conecta con BPROP_001)

* **Sin replaneación:** una entrada `AUTHORIZED` con
  `stale_after_months` no vencido es un input fijo — el mismo
  digest se inyecta siempre, garantizando output determinista
  del agente que lo consume.
* **Con replaneación:** al vencer `stale_after_months`, Fase 0.5
  se re-ejecuta. Solo si la evidencia cambió materialmente se
  actualiza el digest — vía el mismo pipeline de seis fases,
  nunca como edición directa.

## 5. Evaluated Alternatives

### Option A — Pipeline de seis fases formalizado en un TDR único (Selected)
* **Pros:** Reproducible, auditable, conecta los cuatro artefactos
  dispersos (TREQ_045, BREQ_023, BREQ_025, BPROP_001) en un solo
  flujo verificable. Permite a GIR_001/GIR_002 validar que el
  proceso se siguió correctamente.
* **Cons:** Overhead de seis fases para entradas triviales —
  mitigado por Fase 0.5 siendo skip-able cuando no aplica.

### Option B — Proceso informal, cada ciclo de investigación define sus propias fases
* **Pros:** Flexibilidad total.
* **Cons:** Exactamente lo que pasó en la práctica — el proceso
  fue reinventado parcialmente en cada ciclo de esta sesión,
  generando inconsistencia (la versión original de ONT_001-017
  no tuvo Fase 0.5 porque BREQ_025 no existía aún cuando se generaron).

## 6. AREQ Boundary Compliance
* Toda la Fase 1 vía web_search/web_fetch — fuera del boundary
  de adapters internos, es una capacidad de la plataforma de
  Claude, no un adapter del framework.
* Fase 0.5, 2, 3 vía Librarian — atomic writes per TREQ_007.
* Fase 4 vía Orchestrator context assembly — TDR_003.

## 7. Architect Validation
* **Validation required:** NO — formaliza artefactos ya
  AUTHORIZED (TREQ_045, BREQ_023, BREQ_025, BPROP_001) sin
  introducir nuevo adapter o dependencia.
* **Loop 3 triggered:** NO

## 8. Downstream Impact
* **TREQs generated:** TREQ_048 (Ontology Pipeline Implementation
  — las seis fases como código ejecutable)
* **DEV_TICKETs impacted:** Implementación del pipeline completo
* **Retroactive debt documented:** ONT_001-017 generadas antes
  de este TDR no pasaron por Fase 0.5 formal — marcadas para
  re-verificación cuando se ejecute TREQ_048.
