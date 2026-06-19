---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_021
type: BDR
title: "Framework Profile System — Graduated Complexity & Selective Artifact Activation"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_001
    parent_id: BDR_001
  horizontal:
    depends_on: [BDR_005, BDR_016, BDR_020]
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-14T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BDR_021: Framework Profile System
## Graduated Complexity & Selective Artifact Activation

---

## 1. El Problema

El framework en su forma completa (BDR→BREQ→PDR→PREQ→ADR→
AREQ→TDR→TREQ→DEV→TEST, 13 tipos de artefacto, 4 capas)
es la solución correcta para proyectos complejos con equipos,
requisitos de compliance, o decisiones arquitectónicas de
alto riesgo.

Para un developer solo construyendo un side project de fin
de semana, ese mismo framework es overhead que no añade
valor proporcional al esfuerzo.

El riesgo: si el framework solo existe en su forma completa,
el developer del side project no lo usa — y pierde acceso
incluso a las partes que sí le servirían.

---

## 2. La Decisión: Framework Profiles

El framework ofrece tres profiles que el operador selecciona
al inicializar un proyecto. Cada profile define qué artefactos
son obligatorios, cuáles son opcionales, y cuáles no aplican.

El modelo de governance subyacente NO cambia entre profiles —
la jerarquía BDR→DEV_TICKET existe en todos. Lo que cambia
es qué partes de esa jerarquía se activan.

---

## 3. Los Tres Profiles

### PROFILE_LITE — "Quiero planear rápido"
**Casos de uso:** side projects, MVPs de validación,
prototipos, proyectos personales sin team.
**Artefactos obligatorios:**
- BDRs (principios de negocio — simplificados, 1-2 por área)
- DEV_TICKETs (unidad de trabajo — siempre necesaria)
- state.yaml (sesión — siempre necesaria)
**Artefactos opcionales (activables bajo demanda):**
- BREQs, PDRs, ADRs — disponibles si el operador los necesita
**Artefactos no activados:**
- PREQs, AREQs, TREQs, TEST_TICKETs, GIR_001
**Costo de tokens:** mínimo — 2-3 llamadas CLOUD por sesión
**Tiempo estimado de planeación:** 30-60 minutos
**Cuando escalar:** cuando el proyecto crece, el operador
puede migrar a PROFILE_STANDARD sin perder lo ya generado.

### PROFILE_STANDARD — "Quiero hacerlo bien"
**Casos de uso:** productos SaaS, apps con usuarios reales,
proyectos con 1-3 developers, negocios formales.
**Artefactos obligatorios:**
- BDRs + BREQs (negocio completo)
- PDR + PREQs (producto)
- ADRs (arquitectura — sin AREQs)
- DEV_TICKETs + TEST_TICKETs
- GIR simplificado (Planes 1 y 4 solamente)
**Artefactos opcionales:**
- TDRs, TREQs, AREQs — activables para decisiones técnicas
  complejas o cuando el Tech Lead lo considera necesario
**Costo de tokens:** moderado
**Tiempo estimado de planeación:** 2-4 horas
**El default para la mayoría de usuarios.**

### PROFILE_FULL — "Necesito governance completa"
**Casos de uso:** productos enterprise, equipos grandes,
proyectos con compliance regulatorio (EU AI Act, HIPAA,
SOC2), proyectos de alta complejidad técnica.
**Artefactos:** todos los 13 tipos, GIR_001 completo
con cinco planos, ICDs entre capas, ICP completo.
**Costo de tokens:** completo
**Tiempo estimado de planeación:** 1-2 días
**El framework como fue diseñado originalmente.**

---

## 4. Principios de Diseño de Profiles

**P1 — La jerarquía subyacente no cambia.**
BDR→DEV_TICKET existe en los tres profiles. Lo que cambia
es qué capas intermedias son obligatorias. Un proyecto LITE
que tiene BDRs y DEV_TICKETs puede migrar a STANDARD sin
reescribir lo que ya tiene.

**P2 — Los artefactos opcionales siempre están disponibles.**
En PROFILE_LITE, el operador puede pedir un ADR cuando
toma una decisión arquitectónica importante. El framework
lo genera. No es un feature bloqueado — es un feature
no impuesto.

**P3 — La migración hacia arriba es siempre posible.**
LITE → STANDARD → FULL sin pérdida de artefactos previos.
La migración hacia abajo no existe — no puedes "desactivar"
artefactos que ya generaste, solo dejar de generar nuevos.

**P4 — Los tokens escalan con el profile.**
LITE usa principalmente TIER LOCAL. STANDARD usa LOCAL
para tareas mecánicas y CLOUD para decisiones clave.
FULL usa CLOUD para la mayoría de decisiones. El costo
es proporcional al valor que el operador recibe.

**P5 — El profile se declara, no se deduce.**
El operador elige su profile al inicializar el proyecto.
El framework no intenta adivinar qué profile conviene.
Puede sugerir ("Parece un proyecto de una sola persona —
¿PROFILE_LITE es suficiente?") pero el HITL decide.

---

## 5. Impacto en Complejidad Percibida vs Real

El framework PROFILE_LITE tiene la misma base técnica que
PROFILE_FULL. La complejidad interna es idéntica. Lo que
cambia es la superficie que el operador ve:

| Superficie visible | LITE | STANDARD | FULL |
|:------------------|:-----|:---------|:-----|
| Tipos de artefacto que el operador genera activamente | 2 | 6 | 13 |
| Agentes activos | 3 (Business Catalyst, Git Butler, Librarian) | 5 | 9 |
| Pasos de governance obligatorios | 3 | 8 | 15+ |
| Tiempo de onboarding estimado | 15 min | 45 min | 2 horas |

---

## 6. Relación con la Ontología y el Proceso Adversarial

El Profile System responde directamente al riesgo identificado
en ONT_006_adversarial_digest: "la complejidad del framework
es un bloqueante de adopción real." La solución no es
reducir el framework — es hacer que el 80% de los usuarios
no necesiten el 100% del framework para obtener valor.

---

## 7. Consideraciones de Mercado (ONT_009)

PROFILE_LITE es el entry point gratuito. La mayoría de
usuarios de open source entran por aquí. PROFILE_STANDARD
es el "aha moment" — cuando el proyecto crece y el developer
necesita más estructura. PROFILE_FULL es el camino al
enterprise y al revenue.

Este modelo es consistente con el modelo freemium de la
industria (Supabase, PlanetScale, Railway) — la versión
gratuita tiene valor real, no es un trial limitado.
