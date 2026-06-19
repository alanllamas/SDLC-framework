---
id: ONT_012
concept: "atomic-file-writes-posix-temp-rename-python"
layer: "technical"
captured_date: "2026-06-14"
injected_by: "agent"
confidence: "HIGH"
stale_after_months: 36
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_012_original"
    link_reason: "Revisión adversarial — baja prioridad, decisión técnica estable."
---

# ONT_012 — Atomic File Writes (temp-rename)
## Digest Adversarial Verificado — DRAFT pendiente HITL sign-off

---

## FASE 0 — Hipótesis de Falsificación
Esta decisión estaría equivocada si SQLite o una base de datos
embebida provee mejor durabilidad que os.replace() para el
estado de sesión del framework.

---

## FASE 1 — Evidencia

### A — Evidencia a Favor
POSIX estándar garantiza atomicidad de os.rename()/os.replace()
en mismo filesystem. Python 3.3+ os.replace() es cross-platform
(Windows-safe a diferencia de os.rename()). SQLite usa el mismo
principio (WAL) — nuestro approach es consistente con el estado
del arte de bases de datos embebidas.

### B — Evidencia en Contra
ONT_004 adversarial identifica: durabilidad en OS crash es el
riesgo real. os.replace() es atómico a nivel de filesystem pero
no sobrevive un kernel panic entre el write del temp file y el
rename. SQLite WAL tiene durabilidad más robusta.

### C — Alternativa Lateral
SQLite como persistence layer para state.yaml — mismo archivo local,
mejor durabilidad, queries más expresivas. Considerado en ONT_004.

---

## FASE 2 — Síntesis
Para v1.0 Developer Solo con sesiones bounded, os.replace() es
suficiente y simple. La durabilidad adicional de SQLite importa más
en v2.0+ con sesiones largas.

**Veredicto:** CONFIRMADA para v1.0
Implementación: usar os.replace() explícitamente, NamedTemporaryFile
con dir=target.parent para garantizar mismo filesystem. Para v2.0+
evaluar SQLite per ONT_004.

**Fuentes:** POSIX spec, Python docs os.replace(), SQLite WAL documentation.
