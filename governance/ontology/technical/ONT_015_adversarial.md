---
id: ONT_015
concept: "llm-automated-ontology-construction-versioned-evidence"
layer: "technical"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "MEDIUM"
stale_after_months: 12
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_015_original"
    link_reason: "Revisión adversarial."
---

# ONT_015 — LLM-Automated Ontology Construction
## Digest Adversarial Verificado — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
Esta decisión estaría equivocada si la ontología construida
automáticamente por LLMs es demasiado ruidosa o imprecisa para
informar decisiones de governance sin revisión intensiva del HITL.

---

## FASE 1 — Evidencia

### A — Evidencia a Favor
arxiv 2602.01276: LLMs construyen ontologías desde texto con calidad
comparable a expertos humanos en dominios especializados.
LightRAG 2024: KG-based retrieval con 10x menos tokens que GraphRAG
— viable en producción sin overhead de cost.

### B — Evidencia en Contra
La calidad de ontologías LLM-generadas depende de la calidad del
texto fuente. Los artefactos de governance son texto técnico denso —
puede ser difícil para el LLM distinguir decisiones de proceso vs
decisiones de negocio sin contexto adicional.
BREQ_023 (Epistemic Charter): el HITL debe sign-off todos los digests.
Esto significa que la "construcción automática" sigue requiriendo
revisión humana — no es totalmente automática.

### C — Alternativa Lateral
Semi-automático: LLM genera el draft del digest (Fase 2 TREQ_045),
HITL lo revisa. Exactamente lo que ya tenemos con el protocolo
adversarial — la "automatización" es la generación del draft, no la aprobación.

---

## FASE 2 — Síntesis
LLM-generación de drafts es viable y útil. La aprobación HITL per
BREQ_023 sigue siendo obligatoria. El valor real es reducir el tiempo
de generación del draft de horas a minutos — la calidad de la decisión
final depende de la calidad del review del HITL.

**Veredicto:** CONFIRMADA con alcance preciso
LLMs generan drafts de ontología (Fase 2 TREQ_045). HITL aprueba.
La "automatización completa" de la ontología no es posible ni deseable
dado BREQ_023.

**Fuentes:** arxiv 2602.01276, LightRAG 2024, BREQ_023.
