---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_048
type: TREQ
title: "Ontology Pipeline Implementation — Six-Phase Process as Code"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.14.0 — Epistemic Charter & Cost Visibility"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_026
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-18T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# TREQ_048 — Ontology Pipeline Implementation

## 1. Intent & Scope
Implementa las seis fases de TDR_026 como funciones ejecutables,
usando los templates de governance/templates/ONTOLOGY_*.

## 2. Technical Constraints

```python
from pydantic import BaseModel
from typing import Literal

class OntologyPipeline:
    def __init__(self, librarian: "Librarian", web_search: "WebSearchTool"):
        self.librarian = librarian
        self.web_search = web_search

    def run_full_cycle(
        self, concept: str, layer: str,
        informs_artifacts: list[str],
        state: "ProjectState"
    ) -> "OntologyEntry":
        # Fase 0.5
        legal_scan = self._phase_0_5_legal_scan(state)
        if legal_scan.professional_legal_review_required:
            self._notify_hitl_legal_review(legal_scan)

        # Fase 0
        falsification = self._phase_0_falsification_hypothesis(
            concept, informs_artifacts)
        # presentado a HITL para confirmar/ampliar antes de continuar

        # Fase 1
        evidence_for = self._search_direction(concept, "for")
        evidence_against = self._search_direction(concept, "against")
        lateral_alts = self._search_direction(concept, "lateral")
        self._enforce_balance_rule(evidence_for, evidence_against, lateral_alts)

        # Archive sources (Wayback Machine)
        for ev in evidence_for + evidence_against:
            ev.source.url_archived = self._archive_source(ev.source.url_original)

        # Fase 2
        digest = self._synthesize_with_tension(
            falsification, evidence_for, evidence_against, lateral_alts)

        # Write as DRAFT — Fase 3 (sign-off) is separate HITL action
        entry = self.librarian.write_ontology_entry(
            concept=concept, layer=layer, status="DRAFT",
            metadata=..., digest=digest, legal_scan=legal_scan
        )
        return entry

    def _phase_0_5_legal_scan(self, state) -> "LegalContextScan":
        regions = state.config.get("distribution_regions", [])
        vertical = state.config.get("industry_vertical")
        if not regions and not vertical:
            return LegalContextScan(professional_legal_review_required=False, ...)
        # targeted search per BREQ_025 section 3.2
        ...

    def _enforce_balance_rule(self, for_, against, lateral):
        ratio = (len(against) + len(lateral)) / max(len(for_), 1)
        if ratio < 1.0:
            raise IncompleteResearchError(
                "Dirección B+C debe tener al menos tantas fuentes como A "
                f"(actual ratio: {ratio:.2f})")

    def _archive_source(self, url: str) -> str:
        if "arxiv.org" in url:
            return url  # permanente, no requiere archivado
        try:
            resp = httpx.get(f"https://web.archive.org/save/{url}", timeout=30.0)
            return f"https://web.archive.org{resp.headers.get('Content-Location','')}"
        except Exception:
            return url  # non-blocking fallback

    def sign_off(
        self, entry_id: str, hitl_identity: str,
        checks_confirmed: dict[str, bool]
    ) -> "AdapterResult":
        # Fase 3 — verifica los 4 criterios de TREQ_045 antes de AUTHORIZED
        required = ["phase_0_before_search", "b_c_coverage_real",
                    "tension_honest", "verdict_calibrated"]
        if not all(checks_confirmed.get(c, False) for c in required):
            return AdapterResult(success=False,
                error="Sign-off requires all 4 quality checks confirmed")
        self.librarian.update_ontology_status(entry_id, "AUTHORIZED", hitl_identity)
        return AdapterResult(success=True)

    def check_staleness(self, state: "ProjectState") -> list[str]:
        # Fase 5 — corre periódicamente, retorna IDs que vencieron
        entries = self.librarian.scan_ontology_entries(status="AUTHORIZED")
        expired = []
        for e in entries:
            months_elapsed = months_since(e.captured_date)
            if months_elapsed >= e.stale_after_months:
                self.librarian.update_ontology_status(e.id, "DRAFT", None)
                expired.append(e.id)
        return expired
```

## 3. Verification & QA Criteria
* `_enforce_balance_rule()` rechaza síntesis con ratio B+C/A < 1.0
* `_archive_source()` skip arXiv URLs, archiva el resto en Wayback Machine
* `sign_off()` requiere los 4 checks de calidad — ninguno opcional
* `check_staleness()` revierte AUTHORIZED→DRAFT al vencer stale_after_months
* Fase 4 (inyección runtime) solo consume entradas AUTHORIZED —
  verificado en TDR_003 context assembly
* Pipeline completo testeable end-to-end contra las 17 entradas
  ONT_001-017 ya generadas (retroactivo)
