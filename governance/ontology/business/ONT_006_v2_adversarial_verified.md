---
id: ONT_006_v2
concept: "bmad-method-primary-competitor-reference"
layer: "business"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "HIGH"
stale_after_months: 3
status: DRAFT
protocol: TREQ_045
governs_bdr: BDR_021
hitl_signatory: null
---

# ONT_006 — BMAD Method: Competitor Reference + Framework Profile Evidence
## Versión 2 — Con investigación adversarial y evidencia verificada

---

## FASE 0 — Hipótesis de Falsificación
"Esta decisión estaría equivocada si BMAD ya tiene governance
formal equivalente al nuestro, o si la complejidad de nuestro
framework es un bloqueante de adopción mayor que el valor que ofrece."

---

## FASE 1 — Evidencia en las tres direcciones

### A — Evidencia a favor de nuestra diferenciación

**Fuente verificada 1:**
GitHub Issue #2003 — "Structural Gaps and Contradictions of
the BMAD Method V.6 Stable" (marzo 2026)
URL: https://github.com/bmad-code-org/BMAD-METHOD/issues/2003
Hallazgo directo: "The BMAD method, in its current v6 Stable form,
delegates critical decision-making responsibilities to users who
are not trained to take them, while at the same time lacking the
necessary quality controls (safeguards) to prevent development
agents from bypassing the controls themselves."
→ Gap de governance documentado por la propia comunidad de BMAD.

**Fuente verificada 2:**
Ry Walker Research — BMAD Method analysis (febrero 2026)
URL: https://rywalker.com/research/bmad-method
Hallazgo: "12+ agent personas are overhead for solo developers
and teams under ~5; the streamlined Quick Flow path mitigates
but doesn't eliminate this."
→ BMAD mismo reconoce el problema de complejidad para equipos pequeños.
→ BMAD v6.8.0 lanzado mayo 2026 — activo, cerrando gaps continuamente.

**Fuente verificada 3:**
Applied BMAD — Benny Cheung (octubre 2025)
URL: https://bennycheung.github.io/bmad-reclaiming-control-in-ai-dev
Hallazgo: "Enterprise leaders are finding that AI-driven development
comes at a steep price: a loss of governance, traceability, and
architectural integrity."
→ El mercado enterprise identifica governance como el problema.
→ BMAD es la solución que adoptaron — pero aún tiene gaps estructurales.

### B — Evidencia en contra / riesgos

**Riesgo 1 — BMAD evoluciona rápido:**
BMAD pasó de 37K a 49K stars de febrero a junio 2026 (según Ry Walker).
v6.3 a v6.8.0 lanzados en primavera 2026. Web Bundles v1.0 en mayo 2026
empaquetan el framework para Gemini Gems y ChatGPT Custom GPTs con
pricing flat-rate — BMAD está construyendo exactamente el modelo de
plataforma que nosotros consideramos.

**Riesgo 2 — Complejidad como bloqueante confirmado:**
Ry Walker: "Heavier prerequisites — v6 requires Node.js 20.12+,
Python 3.10+, and uv, more setup than skill-file-only frameworks."
Nuestro framework también tiene complejidad — 13 tipos de artefacto
vs PRD único. Ambos sufren el mismo problema, nosotros no lo hemos
resuelto todavía.

**Riesgo 3 — BMAD ya tiene un "Quick Flow":**
BMAD introdujo un Quick Flow para proyectos simples — es su respuesta
al problema de complejidad. Nosotros proponemos Framework Profiles
(BDR_021) como respuesta equivalente. BMAD nos lleva ventaja en la
implementación de esa solución.

### C — Alternativas laterales no consideradas

**Alternativa 1 — BMAD como complemento, no competidor:**
AngelHack DevLabs describe BMAD como "treating AI as a disciplined
participant inside an agile product lifecycle." Si nos posicionamos
como "la capa de governance que BMAD necesita", podemos ser
complementarios en lugar de competidores directamente. Esto reduce
la fricción de adopción porque el usuario ya conoce BMAD.

**Alternativa 2 — Enterprise governance específica:**
Microsoft Open Source Summit (mayo 2026) anuncia "Agent Governance
Toolkit" como primitivas de control-plane abiertas. Hay un movimiento
de estándares abiertos de governance (Agentic AI Foundation, Linux
Foundation). Podríamos posicionarnos dentro de ese ecosistema de
estándares en lugar de competir como producto aislado.

---

## FASE 2 — Síntesis con Tensión

**Tensión central:** BMAD tiene el mercado (49K stars, momentum
activo, Quick Flow implementado) pero gaps estructurales documentados
que nosotros resolvemos. El problema: también estamos construyendo
algo complejo y llegamos después.

**Evidencia a favor:** Los gaps de governance de BMAD son estructurales
— Issue #2003 describe un "conceptual paradox" de diseño, no features
faltantes. Un Quick Flow añadido encima de una arquitectura sin
traceability formal no produce GIR_001. BMAD está construyendo
más velocidad; nosotros estamos construyendo más confiabilidad.

**Evidencia en contra confirmada:** BMAD está lanzando Web Bundles
(plataforma web con pricing flat-rate) — exactamente nuestro plan.
BMAD nos lleva 12+ meses de ventaja en el modelo de plataforma.

**Alternativa lateral más fuerte:** complemento de BMAD o participante
en el ecosistema de estándares (Agentic AI Foundation) podría ser
menos resistencia que competencia directa.

**BDR_021 (Framework Profiles) responde directamente al riesgo de
complejidad** — PROFILE_LITE es nuestra versión del Quick Flow de BMAD,
con la ventaja de que está diseñado desde el principio como un
gradiente, no añadido después.

**Veredicto:** CONFIRMADA_CON_RIESGO
Diferenciador válido (governance formal, GIR, ICDs). Dos riesgos
activos: (1) BMAD cerrando gaps más rápido de lo anticipado — monitoreo
cada 3 meses obligatorio. (2) BMAD ya construyendo la plataforma web —
nuestra ventana para lanzar antes es corta.

---

## Capa 3 — Fuentes verificadas (URLs directas)

| Fuente | URL | Fecha | Hallazgo clave |
|:-------|:----|:------|:---------------|
| BMAD Issue #2003 | https://github.com/bmad-code-org/BMAD-METHOD/issues/2003 | Mar 2026 | Gaps estructurales de governance documentados por la comunidad |
| Ry Walker BMAD Research | https://rywalker.com/research/bmad-method | Feb 2026 | 49K stars, v6.8.0 mayo 2026, Quick Flow como respuesta a complejidad |
| AngelHack DevLabs | https://devlabs.angelhack.com/blog/bmad-method/ | Nov 2025 | BMAD como framework de governance para enterprise |
| Applied BMAD — Benny Cheung | https://bennycheung.github.io/bmad-reclaiming-control-in-ai-dev | Oct 2025 | Enterprise governance gap como problema real |
| Microsoft AAIF | https://opensource.microsoft.com/blog/2026/05/18/from-open-source-to-agentic-systems-microsoft-at-open-source-summit-north-america-2026/ | Mayo 2026 | Agent Governance Toolkit como estándar abierto |
