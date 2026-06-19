---
id: ONT_002
concept: "git-as-governance-backbone-with-decision-records"
layer: "architecture"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "HIGH"
stale_after_months: 12
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_002_original"
    link_reason: "Versión original generada sin protocolo adversarial. Esta versión añade evidencia verificada y búsqueda en tres direcciones per TREQ_045."
---

# ONT_002 — git-as-governance-backbone-with-decision-records
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
*(Formulada antes de buscar evidencia)*

**Esta decisión estaría equivocada si:** Esta decisión estaría equivocada si el ecosistema de ADR tooling ya resuelve lo que hacemos, haciendo nuestro Librarian redundante, o si governance-as-code tiene problemas conocidos de adopción en producción.

**Señales de falsificación que buscamos:**
- ADR tooling existente cubre nuestra jerarquía sin necesidad del Librarian
- Governance-as-code es un paradigma rechazado o que falla en producción

---

## FASE 1 — Evidencia en Tres Direcciones

### A — Evidencia a Favor

**joelparkerhenderson/architecture-decision-record — github.com:**
Fitness functions para ADRs: 'A decision record documents the decision, while a fitness function assures the decision.' GIR_001 es exactamente una fitness function implementada. Validado por la comunidad ADR.

**CIO — Architecture-as-code next frontier (jun 2026):**
Architecture-as-code con fitness functions es 'the next frontier for enterprise governance'. La industria está convergiendo hacia governance como código, no como documentos estáticos.

**GitHub topics: architecture-decision-records:**
Proyecto activo: 'Decision governance for code and the agents editing it — track PRD→ADR→SPEC decisions, map them to files, and gate changes against them. Deterministic, honest CLI + MCP.' Convergencia independiente con nuestro approach.

**Governance Decision Record (GDR) — github.com:**
GDR: 'specification model for computational data governance policies inspired from ADR.' Nuestro framework extiende ADRs exactamente en esta dirección.

### B — Evidencia en Contra y Riesgos

**developersvoice.com — Fitness Functions .NET 2025:**
'Architectural drift: when implemented architecture diverges from planned without deliberate design decision to justify the change.' El problema que GIR_001 resuelve es real y documentado. Pero la solución en .NET usa ArchUnit — no ADRs en Git.

**arc-kit — tractorjuice/arc-kit github:**
ArcKit implementa un Enterprise Architecture Governance Harness completo con MADR v4.0, Wardley maps, roadmaps, risk registers. Es más complejo que nuestro framework y ya existe. ¿Hay overlap no considerado?

**microsoft.com — Declarative YAML agent configuration:**
Microsoft Agent Framework usa YAML declarativo para workflows — confirma el patrón, pero también muestra que frameworks enterprise grandes van más allá de Git+markdown hacia integración con Azure AI Foundry.

### C — Alternativas Laterales no Consideradas
- arc-kit como referencia de implementación — estudiar su approach de 72 comandos de governance antes de construir el Librarian completo.
- MCP como protocolo de acceso al Librarian en lugar de filesystem directo — permitiría que otros agentes externos consumieran el registry de governance.

---

## FASE 2 — Síntesis con Tensión

**Tensión central:** Git + ADRs está validado por la comunidad y por CIO/enterprise como el futuro de governance. El riesgo no es el paradigma — es que arc-kit ya implementa algo similar con más features. El Librarian que construimos podría estar reinventando partes de arc-kit.

**Veredicto:** CONFIRMADA_CON_RIESGO
**Razón:** Git backbone y fitness functions (GIR_001) son el estado del arte. Riesgo: arc-kit existe y puede tener overlap. Acción: estudiar arc-kit antes de construir el Librarian completo para no reinventar lo ya resuelto.

---

## Capa 3 — Fuentes Verificadas

| Fuente | URL | Fecha |
|:-------|:----|:------|
| joelparkerhenderson/architecture-decision-record | https://github.com/architecture-decision-record/architecture-decision-record | 2024 |
| CIO — Architecture-as-code | https://www.cio.com/article/4184567/architecture-as-code-is-the-next-frontier-for-enterprise-governance.html | Jun 2026 |
| arc-kit Enterprise Governance Harness | https://github.com/tractorjuice/arc-kit | 2026 |
| GitHub ADR topics | https://github.com/topics/architecture-decision-records | 2026 |
