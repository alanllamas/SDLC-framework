---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_045_amendment_001
type: TREQ_AMENDMENT
title: "TREQ_045 Amendment — Fase 0.5: Legal Context Scan"
status: AUTHORIZED
relationships:
  vertical:
    parent_id: TREQ_045
    governing_bdr: BDR_018
  horizontal:
    depends_on: [BREQ_025]
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-18T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# TREQ_045 Amendment 001 — Fase 0.5: Legal Context Scan

## 1. Cambio

TREQ_045 (Adversarial Research Protocol) se extiende con una
Fase 0.5 que precede a la Fase 0 (Hipótesis de Falsificación).

## 2. Implementación

```python
class LegalContextScan(BaseModel):
    scan_date: str
    distribution_regions: list[str]
    industry_vertical: str | None
    applicable_frameworks: list[dict]
    ip_considerations: list[dict]
    professional_legal_review_required: bool
    review_priority: str
    disclaimer: str

def run_legal_context_scan(
    state: "ProjectState",
    decision_id: str,
    librarian: "Librarian"
) -> LegalContextScan:
    """
    Ejecuta Fase 0.5 per BREQ_025.
    Corre ANTES de Fase 0 (hipótesis de falsificación).

    Triggers:
    - state.config.distribution_regions está definido
    - state.config.industry_vertical está definido
    - decision_id involucra datos de usuarios, logs persistentes,
      modelos cloud externos, retención de artefactos, o
      procesamiento automatizado

    Si ningún trigger aplica: retorna LegalContextScan vacío
    con professional_legal_review_required = false.
    """
    regions = state.config.get("distribution_regions", [])
    vertical = state.config.get("industry_vertical")

    if not regions and not vertical:
        return LegalContextScan(
            scan_date=today_iso(),
            distribution_regions=[],
            industry_vertical=None,
            applicable_frameworks=[],
            ip_considerations=[],
            professional_legal_review_required=False,
            review_priority="N/A",
            disclaimer="No distribution regions or industry vertical configured."
        )

    # Agent runs targeted search per BREQ_025 section 3.2
    # Results assembled into LegalContextScan payload
    # HITL notified if professional_legal_review_required = true
    ...
```

## 3. Preservación de fuentes (Wayback Machine)

El comando `/ontology add` extiende su implementación:

```python
def archive_source(url: str) -> str:
    """
    Submits URL to Wayback Machine Save API.
    Returns archived URL or original if archiving fails.
    Failure is non-blocking — logs warning, continues.
    """
    try:
        resp = httpx.get(
            f"https://web.archive.org/save/{url}",
            timeout=30.0
        )
        archived = resp.headers.get("Content-Location", "")
        if archived:
            return f"https://web.archive.org{archived}"
    except Exception:
        pass
    return url  # fallback to original, log warning

def add_ontology_entry(
    entry: OntologyEntry,
    librarian: "Librarian"
) -> "AdapterResult":
    for source in entry.sources:
        if source.url and not source.url.startswith("https://arxiv"):
            source.url_archived = archive_source(source.url)
            source.archived_date = today_iso()
    librarian.write_ontology_entry(entry)
    return AdapterResult(success=True)
```

## 4. state.yaml extension

```yaml
project:
  distribution_regions: []  # e.g. ["EU", "MX", "US"]
  industry_vertical: null   # e.g. "health", "finance", "government"
```

Configurable durante bootstrap (Path A, BREQ_017 Step 2 —
Operator Profile Load) o en cualquier momento via:
`/project set distribution_regions EU,MX`
`/project set industry_vertical health`

## 5. Criterios de Verificación

- Fase 0.5 corre cuando distribution_regions o industry_vertical
  están configurados — siempre antes de Fase 0.
- professional_legal_review_required = true genera notificación
  visible al HITL, nunca silenciosa.
- Fuentes web archivadas en Wayback Machine en el momento de
  `/ontology add` — URL archivado registrado en Capa 3.
- arXiv y DOIs: no requieren archivado — permanentes por diseño.
- Fase 0.5 vacía (sin triggers) tarda <1s — no bloquea workflow.
