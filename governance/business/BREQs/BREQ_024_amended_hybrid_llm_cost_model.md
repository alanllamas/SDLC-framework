---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_024
type: BREQ
title: "Hybrid LLM Cost Model — Local vs Cloud, Pre-op Estimates & Budget Visibility"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_009
    parent_id: BDR_009
  horizontal:
    depends_on: [BREQ_011, BREQ_023]
  cross_plane:
    architecture_adr: [ADR_001]
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-14T00:00:00Z" }
  amendment_note: >
    2026-06-14: Added pre-operation cost estimates, real-time
    accumulator, and user-configured budget per session/project.
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BREQ_024: Hybrid LLM Cost Model
## Local vs Cloud, Pre-Operation Estimates & Budget Visibility

---

## 1. Problema

El framework requiere múltiples llamadas a LLM por sesión.
Con el usuario trayendo su propia API key, el costo
acumulado puede ser un bloqueante de adopción — especialmente
para el Developer Solo que quiere evaluar el framework
antes de comprometerse con un gasto recurrente.

Tres problemas específicos:
1. No todas las tareas requieren un modelo de producción.
2. El usuario no sabe cuánto va a gastar ANTES de que ocurra.
3. El usuario no sabe cuánto ha gastado mientras trabaja.

---

## 2. Clasificación de Tareas (LOCAL vs CLOUD)

### TIER LOCAL — Ollama (costo $0)
Tareas mecánicas o de bajo riesgo epistémico:
- Formateo y validación de YAML/Markdown
- Cálculo de IDs secuenciales y slugs
- Parsing de front-matter de artefactos
- Clasificación simple (/log opciones A-E)
- Construcción del dependency graph (traversal puro)
- Fase 0 del protocolo adversarial (hipótesis de falsificación)
- Síntesis de resultados de búsqueda web (Fase 1 TREQ_045)
- Validación de commit message prefix

### TIER CLOUD — Claude API (tokens del usuario)
Tareas que requieren juicio epistémico real:
- Elicitación de requerimientos (Business Catalyst)
- Evaluación de decisiones arquitectónicas (System Architect)
- Generación de TDRs con Evaluated Alternatives
- Síntesis con tensión de evidencia adversarial (Fase 2 TREQ_045)
- Chronicle generation (Agent 7) — requiere comprensión real de código
- Compliance Report (Agent 9) — juicio, no checklist
- Cualquier tarea cuyo output el HITL va a firmar

### TIER AMBIGUO — Configurable en .butler.env
- Release Notes (Agent 8): suficiente con LOCAL para proyectos simples
- Product Delivery translation: depende de complejidad
- Sugerencias de Documentation Assignment

```
# .butler.env
AMBIGUOUS_TIER=local    # default conservador (costo $0)
# AMBIGUOUS_TIER=cloud  # mayor calidad, costo real
```

---

## 3. Pre-Operation Cost Estimate

Antes de ejecutar cualquier llamada CLOUD, el Orchestrator
calcula y presenta al operador:

```
⚡ Operación CLOUD: [tipo de operación]
   Input estimado:  [N] tokens  (~$X.XXX)
   Output estimado: ~[N] tokens (~$X.XXX)
   ─────────────────────────────────────
   Total estimado:  ~$X.XXX
   Acumulado sesión:   $X.XX  de $[budget_sesion] configurado
   Acumulado proyecto: $X.XX  de $[budget_proyecto] configurado
   ─────────────────────────────────────
   ¿Continuar?  [Y] Cloud  [L] Local  [N] Cancelar
```

### Reglas del estimado
* Input real: conocido antes de la llamada (ya tokenizado
  para el ceiling de 9,500 tokens — ADR_003).
* Output estimado: promedio histórico por tipo de tarea,
  calibrado con historial del proyecto. Valores iniciales:

  | Tipo de tarea | Output estimado |
  |:-------------|:----------------|
  | BDR/BREQ generation | 800 tokens |
  | PDR/PREQ generation | 600 tokens |
  | ADR/AREQ generation | 1,200 tokens |
  | TDR generation | 1,500 tokens |
  | Chronicle (Agent 7) | 800 tokens |
  | Compliance Report (Agent 9) | 600 tokens |
  | Elicitation turn | 400 tokens |
  | Ontology digest (Fase 2) | 500 tokens |

* El estimado se muestra con "~" para comunicar que es
  aproximado. Después de la llamada real, el delta entre
  estimado y real se usa para refinar el promedio histórico.

### Umbral de confirmación configurable
```
# .butler.env
COST_CONFIRM_THRESHOLD=0.05   # Pedir confirmación si >$0.05
                               # 0.00 = siempre pedir
                               # 999  = nunca pedir
```
Operaciones bajo el umbral proceden sin interrupción.
Operaciones sobre el umbral siempre presentan el estimado.

---

## 4. Real-Time Budget Accumulator

El Orchestrator mantiene tres contadores en state.yaml:

```yaml
session:
  cost_tracker:
    session_cloud_tokens_in: 0
    session_cloud_tokens_out: 0
    session_cloud_cost_usd: 0.000
    project_cloud_cost_usd: 0.000  # persiste entre sesiones
    budget_session_usd: null       # null = sin límite configurado
    budget_project_usd: null       # null = sin límite configurado
```

El operador configura sus budgets en .butler.env:
```
SESSION_BUDGET_USD=5.00     # límite por sesión de trabajo
PROJECT_BUDGET_USD=50.00    # límite total del proyecto
```

### Comportamiento al acercarse al límite:
* **80% del budget:** advertencia informativa en próxima
  operación CLOUD — "Llevas el 80% de tu budget de sesión."
* **95% del budget:** confirmación obligatoria independiente
  del COST_CONFIRM_THRESHOLD — "Estás cerca del límite.
  ¿Continuar?"
* **100% del budget:** el Orchestrator ofrece tres opciones:
  - Continuar sin límite (override documentado en Ledger)
  - Cambiar operaciones pendientes a LOCAL donde sea posible
  - Pausar sesión y reanudar en nueva sesión con budget fresco

### Comando de consulta en cualquier momento:
```
/cost
→ Sesión actual:   $0.24 (48% de $5.00 configurado)
   Tokens in:  80,200  |  Tokens out: 16,100
   Proyecto:   $1.87 (4% de $50.00 configurado)
   Estimado para completar milestone actual: ~$0.60
```

El estimado de "completar milestone actual" se calcula
contando DEV_TICKETs pendientes × costo promedio por ticket.

---

## 5. Visibilidad de Tokens Disponibles

La API de Anthropic no expone saldo en tiempo real.
El framework approxima "tokens disponibles" como:

```
tokens_restantes_sesion = (budget_sesion - costo_acumulado_sesion)
                          / precio_por_token_output
```

Esto es tokens restantes contra el BUDGET del usuario,
no contra su saldo real de cuenta — que es más útil en
práctica porque el saldo de cuenta es dinero ilimitado
(hasta la tarjeta), no una restricción operativa real.

Para el rate limit de la API (requests por minuto):
el Orchestrator detecta rate limit errors y espera
automáticamente con backoff exponencial, informando
al operador: "Rate limit alcanzado — reintentando en Xs."

---

## 6. HybridLLMAdapter — Contrato de Implementación

```python
class HybridLLMAdapter:
    local: LLMAdapter   # OllamaAdapter — $0
    cloud: LLMAdapter   # ClaudeAdapter — tokens reales

    def estimate_cost(
        self,
        prompt: str,
        task_type: str
    ) -> CostEstimate:
        tokens_in = count_tokens(prompt)
        tokens_out_avg = TASK_OUTPUT_AVERAGES[task_type]
        return CostEstimate(
            tokens_in=tokens_in,
            tokens_out_estimated=tokens_out_avg,
            cost_usd=(tokens_in * PRICE_IN + tokens_out_avg * PRICE_OUT)
        )

    def invoke(
        self,
        task_tier: Literal["LOCAL", "CLOUD", "AMBIGUOUS"],
        prompt: str,
        task_type: str,
        state: ProjectState,
        confirm_fn: Callable | None = None
    ) -> AdapterResult:
        if task_tier == "LOCAL":
            return self.local.invoke(prompt)

        if task_tier == "AMBIGUOUS":
            tier = state.config.ambiguous_tier_preference
            if tier == "local":
                return self.local.invoke(prompt)

        # CLOUD path
        estimate = self.estimate_cost(prompt, task_type)

        if estimate.cost_usd > state.config.cost_confirm_threshold:
            if confirm_fn:
                confirmed = confirm_fn(estimate, state.cost_tracker)
                if not confirmed:
                    return AdapterResult(
                        success=False,
                        error="CLOUD operation cancelled by operator"
                    )

        result = self.cloud.invoke(prompt)

        # Update real-time tracker
        state.cost_tracker.update(
            tokens_in=result.tokens_in_real,
            tokens_out=result.tokens_out_real
        )
        # Refine task average for future estimates
        self._update_task_average(task_type, result.tokens_out_real)

        return result
```

---

## 7. Métricas de Éxito

- M1: >70% de llamadas LLM por sesión van a TIER LOCAL.
- M2: Costo Cloud por ciclo completo de planeación
  (Business→Technical, proyecto mediano) < $2 USD.
- M3: 0% de sorpresas de costo — el operador siempre
  vio el estimado antes de operaciones sobre el umbral.
- M4: Estimados de costo tienen error < 30% vs costo real,
  mejorando con historial del proyecto.
- M5: /cost disponible en cualquier momento de la sesión,
  responde en < 100ms (cálculo local, no API call).
