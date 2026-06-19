---
id: ONT_002
concept: "git-as-governance-backbone-with-decision-records"
layer: "architecture"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_002_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_002 — git-as-governance-backbone-with-decision-records
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
ADR tooling existente cubre nuestra jerarquía sin necesidad del Librarian propio.

## Tensión Central
Git + ADRs con fitness functions (GIR_001) está validado como el paradigma correcto. Riesgo: arc-kit (github.com/tractorjuice/arc-kit) ya implementa un Enterprise Architecture Governance Harness con funcionalidad similar al Librarian.

## Evidencia a Favor
GIR_001 = fitness function — validado por comunidad ADR. CIO: architecture-as-code es "the next frontier for enterprise governance".

## Evidencia en Contra
arc-kit implementa ADRs, dependency tracking, roadmaps, risk registers — overlap real con el Librarian que planeamos construir.

## Alternativa Lateral
arc-kit como base del Librarian en lugar de construir desde cero. Evaluar compatibilidad con nuestra jerarquía BDR→DEV_TICKET.

## Veredicto: CONFIRMADA_CON_RIESGO
Git backbone correcto. Acción antes de construir Librarian: estudiar arc-kit para identificar overlap y decidir adoptar vs construir.

## Fuentes Verificadas
- [ADR GitHub Community](https://adr.github.io) — 2024
- [arc-kit Enterprise Governance Harness](https://github.com/tractorjuice/arc-kit) — 2026
- [CIO — Architecture-as-code](https://www.cio.com/article/4184567/architecture-as-code-is-the-next-frontier-for-enterprise-governance.html) — Jun 2026
