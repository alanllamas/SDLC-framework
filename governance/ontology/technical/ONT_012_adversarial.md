---
id: ONT_012
concept: "atomic-file-writes-posix-temp-rename-python"
layer: "technical"
captured_date: "2026-06-18"
injected_by: "agent"
status: DRAFT
protocol: TREQ_045
hitl_signatory: null
history:
  supersedes_document:
    document_id: "ONT_012_original"
    link_reason: "Versión original sin protocolo adversarial. Esta versión añade evidencia verificada per TREQ_045."
informs_artifacts: []
---

# ONT_012 — atomic-file-writes-posix-temp-rename-python
## Digest Adversarial Verificado (TREQ_045) — DRAFT pendiente HITL sign-off

---

## Hipótesis de Falsificación
SQLite provee mejor durabilidad sin overhead significativo para el caso de uso v1.0.

## Tensión Central
os.replace() es atómico a nivel filesystem pero no garantiza durabilidad en kernel panic. SQLite WAL es más robusto. Para v1.0 Developer Solo con sesiones bounded, os.replace() es suficiente.

## Evidencia a Favor
POSIX garantiza atomicidad de os.replace() en mismo filesystem. Python 3.3+ es cross-platform.

## Evidencia en Contra
Sin durabilidad garantizada en OS crash. SQLite WAL resuelve esto nativamente.

## Alternativa Lateral
SQLite como persistence layer — mismo archivo local, sin infraestructura, mejor durabilidad.

## Veredicto: CONFIRMADA v1.0
CONFIRMADA v1.0. Implementación: os.replace() + NamedTemporaryFile(dir=target.parent). Para v2.0+ evaluar SQLite per ONT_004.

## Fuentes Verificadas
- [Python docs os.replace()](https://docs.python.org/3/library/os.html#os.replace) — 2024
- [SQLite WAL documentation](https://www.sqlite.org/wal.html) — 2024
