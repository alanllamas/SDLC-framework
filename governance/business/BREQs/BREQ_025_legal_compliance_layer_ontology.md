---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_025
type: BREQ
title: "Legal Compliance Layer in Ontological Research & Agent Decision-Making"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_018
    parent_id: BDR_018
  horizontal:
    depends_on: [BREQ_023, TREQ_045]
  cross_plane:
    architecture_adr: [ADR_004]
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-18T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BREQ_025: Legal Compliance Layer
## Ontological Research & Agent Decision-Making

---

## 1. El Problema

El proceso de investigación adversarial (TREQ_045) busca evidencia
técnica, académica, y de mercado. No tiene ningún mecanismo para
identificar restricciones legales que aplican a las decisiones que
el framework toma — o que el framework ayuda al HITL a tomar.

Un agente puede producir un ADR técnicamente correcto que es
ilegalmente inutilizable en el mercado objetivo del proyecto.

Ejemplos de lo que se puede perder sin este BREQ:
- HIPAA (salud, EE.UU.): restricciones sobre qué datos puede
  procesar un LLM externo — impacta directamente qué modelos
  cloud son permitidos en el Hybrid LLM Cost Model (BREQ_024).
- GDPR (Europa): retención de logs de decisiones de agentes,
  derecho al olvido, data residency — impacta el Ledger de
  eventos (TDR_005) y el Project Registry (ADR_005).
- EU AI Act: sistemas de alto riesgo requieren conformidad que
  va más allá del HITL — impacta cómo se certifica el framework.
- FedRAMP (gobierno EE.UU.): solo modelos cloud en lista aprobada
  — impacta el ClaudeAdapter y potencialmente el OllamaAdapter.
- SOC 2 / ISO 27001: impactan cómo se almacenan los artefactos
  de governance y quién puede acceder.
- Propiedad intelectual: las evidencias capturadas en la ontología
  tienen restricciones de copyright que varían por fuente y por
  territorio de distribución.

---

## 2. La Decisión

Toda investigación ontológica (TREQ_045) debe incluir una
**Fase 0.5 — Legal Context Scan** ejecutada ANTES de la búsqueda
de evidencia técnica.

Esta fase no reemplaza asesoría legal profesional — la identifica
como necesaria cuando aplica, y documenta los riesgos para que
el HITL pueda buscar esa asesoría.

---

## 3. Fase 0.5 — Legal Context Scan

### 3.1 Trigger
Se ejecuta automáticamente cuando:
- El proyecto tiene un `industry_vertical` configurado en state.yaml
  (salud, finanzas, gobierno, educación, energía)
- El proyecto tiene una `distribution_region` configurada
  (EU, US, LATAM, APAC, etc.)
- La decisión bajo análisis involucra: datos de usuarios, logs
  persistentes, modelos cloud externos, retención de artefactos,
  o procesamiento automatizado sin revisión humana

### 3.2 Qué busca el agente

**Por región de distribución:**
- EU: EU AI Act (riesgo alto/limitado/mínimo), GDPR (datos personales
  en logs y artefactos), NIS2 (si aplica infraestructura)
- EE.UU.: sector-específico (HIPAA, FERPA, GLBA, CCPA/CPRA),
  FedRAMP si hay gobierno, state-level AI laws (Colorado AI Act,
  Texas AI Act, Illinois, etc.)
- LATAM: LGPD (Brasil), Ley Fintech (México), leyes de datos
  por país del proyecto
- Global: ISO 42001 (AI management systems), NIST AI RMF

**Por vertical de industria:**
- Salud: HIPAA, HL7 FHIR, restricciones de LLM externo para PHI
- Finanzas: SOX, PCI-DSS, restricciones de audit trail
- Gobierno: FedRAMP, requisitos de soberanía de datos
- Legal: restricciones de privilegio abogado-cliente en logs
- Educación: FERPA, COPPA si hay menores

**Por tipo de decisión:**
- Procesamiento de datos con LLM cloud: verificar data residency
  y términos de servicio del proveedor del modelo
- Logs persistentes de decisiones: verificar retención mínima/máxima
  y derecho de acceso/eliminación
- Artefactos de governance como evidencia legal: verificar admisibilidad
  y requisitos de integridad (cadena de custodia)
- Ontología con fragmentos de terceros: verificar fair use vs
  licencia requerida por territorio

### 3.3 Output de Fase 0.5

```yaml
legal_context:
  scan_date: "YYYY-MM-DD"
  distribution_regions: ["EU", "MX"]
  industry_vertical: "saas_general"
  applicable_frameworks:
    - id: "EU_AI_ACT"
      risk_classification: "MINIMAL"  # MINIMAL / LIMITED / HIGH / UNACCEPTABLE
      applicable_articles: ["Art. 14", "Art. 52"]
      implication: "Cooling-off + sign-off + Ledger cumplen Art. 14 para sistema minimal risk"
      hitl_action_required: false
    - id: "GDPR"
      applicable: true
      implication: "Ledger de eventos contiene timestamps y decisiones — puede constituir datos personales si el operador es identificable. Retención: verificar con DPA."
      hitl_action_required: true
      action: "Definir política de retención del Ledger antes de distribución en EU"
  ip_considerations:
    ontology_sources_with_restricted_use:
      - source_id: "bmad-issue-2003"
        license: "GitHub ToS — contenido de usuarios"
        fair_use_assessment: "PROBABLE para análisis interno"
        distribution_risk: "LOW — fragmento de discusión pública"
      - source_id: "pwc-report-2026"
        license: "Copyright PwC — todos los derechos reservados"
        fair_use_assessment: "PROBABLE para citación con atribución"
        distribution_risk: "MEDIUM — verificar ToS antes de incluir fragmentos en material público"
  professional_legal_review_required: true
  review_priority: "BEFORE_PUBLIC_DISTRIBUTION"
  disclaimer: >
    Este análisis es informativo, no constituye asesoría legal.
    El HITL debe buscar asesoría legal profesional antes de
    distribuir el framework en los territorios identificados.
```

---

## 4. Integración con TREQ_045

TREQ_045 (Adversarial Research Protocol) se extiende con una nueva
Fase 0.5 que precede a la Fase 0 (Hipótesis de Falsificación):

```
FASE 0.5 — Legal Context Scan (nueva, BREQ_025)
    ↓
FASE 0 — Hipótesis de Falsificación (existente, TREQ_045)
    ↓
FASE 1 — Búsqueda en Tres Direcciones (existente, TREQ_045)
    ↓
FASE 2 — Síntesis con Tensión (existente, TREQ_045)
    ↓
FASE 3 — Sign-off HITL (existente, TREQ_045)
```

La Fase 0.5 alimenta la Fase 1 Dirección B (evidencia en contra)
con riesgos legales identificados — una decisión puede ser
técnicamente correcta y legalmente problemática en ciertos mercados.

---

## 5. Preservación de Evidencia Ontológica

Como corolario de este BREQ, toda fuente capturada en la ontología
requiere un mecanismo de preservación que garantice su disponibilidad
futura para propósitos de audit y compliance:

**Para fuentes web (blogs, artículos, issues):**
- Archivar en Wayback Machine (archive.org) en el momento de captura
- Registrar el URL archivado en la Capa 3 junto al URL original
- Capturar el fragmento textual exacto que respalda el hallazgo

**Para papers académicos:**
- DOI + arXiv ID son permanentes — URL actual es suficiente
- Para papers detrás de paywall: capturar abstract + cita formal

**Para normas y regulaciones:**
- URL oficial del organismo regulador
- Versión/fecha del documento (las normas se actualizan)
- Nota de que la norma puede ser enmendada — stale_after_months: 6

**Implementación:** el comando `/ontology add` (definido en BPROP_001)
ejecuta automáticamente el archivado en Wayback Machine antes de
guardar la entrada. El URL archivado se almacena en:
```yaml
source:
  url_original: "https://example.com/article"
  url_archived: "https://web.archive.org/web/20260618/https://example.com/article"
  archived_date: "2026-06-18"
  fragment_captured: "exact text excerpt that supports the finding"
```

---

## 6. Aplicación al Framework Mismo

El framework como producto está sujeto a estas mismas
consideraciones legales en su distribución:

**EU AI Act clasificación del framework:**
El framework es un sistema de AI que asiste decisiones de governance
de software. La clasificación depende del uso: si se usa en sistemas
de alto riesgo (RRHH, crédito, infraestructura crítica), hereda esa
clasificación. Para uso general de desarrollo de software: minimal risk.
**Acción:** documentar clasificación recomendada en README y en
los términos de uso del framework.

**GDPR en el Ledger de eventos:**
El Ledger registra timestamps, identidad del signatario, y decisiones.
Si el operador es una persona identificable (no solo un proyecto),
el Ledger puede constituir datos personales bajo GDPR.
**Acción:** antes de distribución en EU, definir política de
retención y mecanismo de anonimización o eliminación del Ledger.

**Copyright en la ontología distribuida:**
Si el framework distribuye entradas de ontología con fragmentos
capturados de fuentes de terceros, los fragmentos deben cumplir
fair use en cada territorio de distribución.
**Acción:** revisión legal antes de distribución pública de
cualquier ontología pre-cargada con el framework.

---

## 7. Métricas de Éxito

- M1: 100% de proyectos con industry_vertical o distribution_region
  configurados ejecutan Fase 0.5 antes de cualquier investigación.
- M2: 100% de fuentes web en la ontología tienen URL archivado en
  Wayback Machine.
- M3: 0% de fragmentos capturados sin evaluación de fair use en
  la Capa 3.
- M4: professional_legal_review_required = true produce una
  notificación visible al HITL — no se entierra en el YAML.
