---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BDR_022
type: BDR
title: "Business Model — Open Framework + Commercial Platform Strategy"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_001
    parent_id: BDR_001
  horizontal:
    depends_on: [BDR_021]
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-14T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BDR_022: Business Model
## Open Framework + Commercial Platform Strategy

---

## 1. La Decisión de Negocio

El framework se libera como open source (MIT license).
La plataforma — interfaz, hosting, y servicios de valor
añadido — es el producto comercial.

Esta decisión responde directamente a ONT_009
(market timing adversarial): el mercado de 2026 está
en fase de adopción gratuita. Revenue viene de 2027-2028
cuando la industria esté lista para pagar por governance
formal. La estrategia correcta es construir adopción ahora,
monetizar después.

---

## 2. Los Dos Productos

### PRODUCTO 1 — SDLC Framework (Open Source, MIT)
**Qué es:** El framework completo — agents, adapters,
Librarian, Git Butler, GIR engine, todos los protocolos —
como librería Python instalable.

**Quién lo usa:** Developers técnicos que quieren correr
el framework en su propia infraestructura, integrar con
sus herramientas existentes, o contribuir al proyecto.

**Cómo se monetiza:** No directamente. Es el mecanismo
de adopción y construcción de comunidad. El framework
open source hace crecer el ecosistema que eventualmente
necesita la plataforma.

**Modelo análogo:** LangChain (framework), Supabase
(Postgres + interfaz), Temporal (engine open source).

---

### PRODUCTO 2 — SDLC Platform (Comercial)
**Qué es:** Una interfaz — web, desktop, o ambas —
que corre el framework con toda la experiencia de
operador integrada: gestión de proyectos, visualización
del artifact graph, dashboard de costos en tiempo real,
Mermaid diagrams renderizados, historiales de sesión,
y eventualmente colaboración multi-usuario.

**Quién lo usa:** El mismo developer que conoció el
framework en open source pero quiere la experiencia
productizada, o empresas que necesitan la plataforma
sin instalar nada.

**Cómo se monetiza:**
- Freemium: PROFILE_LITE gratis, PROFILE_STANDARD
  y FULL con pricing por proyecto o suscripción mensual.
- Enterprise: PROFILE_FULL + SLA + soporte + compliance
  reports exportables para auditores.

---

## 3. Timeline de Mercado (basado en ONT_009)

**2026 — Build & Validate (sin revenue objetivo):**
- Framework open source disponible, PROFILE_LITE funcional
- Early adopters: developers técnicos que quieren probar
- Objetivo: feedback real, iteración rápida, primeros
  contributors open source
- Métrica clave: adopción (installs, GitHub stars, PRs)

**2027 — Platform Alpha (revenue exploratorio):**
- Plataforma web en alpha privada con early adopters
- Primeros pagos experimentales — entender disposición
  a pagar y qué features justifican precio
- Objetivo: product-market fit para la plataforma
- Métrica clave: retención y NPS de la plataforma

**2028 — Scale (revenue objetivo):**
- Plataforma pública con pricing estable
- Enterprise tier con compliance formal
- Objetivo: ARR sostenible
- Métrica clave: MRR, churn, expansion revenue

---

## 4. El Riesgo Principal (ONT_009 adversarial)

El Developer Solo puede no ser el pagador. Puede usar
el framework gratuito indefinidamente y nunca convertirse
en cliente de la plataforma.

**Mitigación:** El valor de la plataforma sobre el
framework CLI tiene que ser suficientemente claro para
que la conversión tenga sentido. Las features que solo
la plataforma puede ofrecer (colaboración multi-usuario,
dashboard de costos centralizado, exportación de
compliance reports, integración con CI/CD) son el
"pull" que mueve al usuario de CLI a plataforma.

El segmento que paga primero probablemente sea
PROFILE_STANDARD/FULL para equipos pequeños (2-5
personas), no el Developer Solo individual.

---

## 5. La Pregunta No Resuelta (requiere validación)

¿Qué convierte a un usuario de open source en un cliente
de plataforma? Esta pregunta no puede responderse con
análisis — requiere conversaciones reales con early adopters.

Esta es la razón principal para lanzar open source primero:
los early adopters del framework gratuito son los que
van a decir exactamente qué features de la plataforma
justificarían que paguen.

**Acción requerida antes de construir la plataforma:**
50 conversaciones con developers que hayan usado el
framework por al menos 2 semanas. Esta validación va
antes de cualquier línea de código de la plataforma.

---

## 6. Liberación del Framework "en Concepto"

Una variante que merece evaluación: liberar primero la
documentación del framework (BDRs, ADRs, protocolos) como
un estándar abierto — antes de que el código esté listo.
Esto permite que la comunidad valide el approach, proponga
mejoras, y construya expectativa, sin que el código
tenga que estar perfecto.

Esto es lo que hizo OpenTelemetry (spec primero, implementaciones
después) y varios estándares de API governance.

**Ventaja:** comunidad antes de código, retroalimentación
más temprana, diferencia el "estándar" (abierto) del
"software" (potencialmente comercial).

**Riesgo:** sin código de referencia, el estándar puede
ser difícil de evaluar para developers que quieren ver
antes de comprometerse.
