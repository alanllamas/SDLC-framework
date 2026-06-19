---
extends_schema: "/governance/templates/master_metadata.yaml"
id: BREQ_024
type: BREQ
title: "Hybrid LLM Cost Model — Local vs Cloud Task Classification"
status: AUTHORIZED
relationships:
  vertical:
    governing_bdr: BDR_009
    parent_id: BDR_009
  horizontal:
    depends_on: [BREQ_011, BREQ_023]
  cross_plane:
    architecture_adr: [ADR_001]
    technical_schema: []
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Business Catalyst", timestamp: "2026-06-14T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# BREQ_024: Hybrid LLM Cost Model

## 1. Problema
El framework requiere múltiples llamadas a LLM por sesión.
Con Claude API como único proveedor y el usuario trayendo
su propia API key, el costo acumulado puede ser un
bloqueante de adopción — especialmente para el Developer
Solo que tiene presupuesto limitado o quiere evaluar el
framework antes de comprometerse con un gasto recurrente.

Adicionalmente, no todas las tareas del framework requieren
la calidad de un modelo de producción. Algunas son
mecánicas o de bajo riesgo epistémico — usar Claude API
para ellas es overhead injustificado.

---

## 2. Clasificación de Tareas

### TIER LOCAL — Modelo local via Ollama (costo $0)
Tareas mecánicas, deterministas, o de bajo riesgo
epistémico donde la calidad del modelo local es suficiente:

- Formateo y validación de YAML/Markdown
- Cálculo de IDs secuenciales
- Parsing de front-matter de artefactos
- Clasificación simple (/log opciones A-E)
- Generación de slugs y nombres de archivo
- Construcción del dependency graph
  (es traversal, no razonamiento)
- Fase 0 del protocolo adversarial
  (hipótesis de falsificación — razonamiento estructurado
  pero no requiere calidad de producción)
- Síntesis de resultados de búsqueda web para Fase 1

### TIER CLOUD — Claude API (costo real, tokens del usuario)
Tareas que requieren juicio epistémico, creatividad
calibrada, o razonamiento complejo sobre contexto
de governance:

- Elicitación de requerimientos (Business Catalyst)
- Evaluación de decisiones arquitectónicas (System Architect)
- Generación de TDRs con Evaluated Alternatives reales
- Síntesis con tensión de evidencia adversarial (Fase 2)
- OPERATIONAL_MANDATE distillation check
- Compliance Report de Agent 9 (juicio, no checklist)
- Chronicle generation de Agent 7 (requiere comprensión
  de código, no solo formateo)
- Cualquier tarea donde el output informa una decisión
  que el HITL va a firmar

### TIER AMBIGUO — Decisión del operador en .butler.env
Tareas donde la calidad correcta depende del proyecto
y las preferencias del usuario:

- Generación de Release Notes (Agent 8) — puede ser
  suficiente con modelo local para proyectos simples
- Validación de commit messages
- Sugerencias de Documentation Assignment

---

## 3. Implementación via LLMAdapter

El LLMAdapter (ADR_001) ya soporta intercambio de
implementación via .butler.env. BREQ_024 formaliza que
el LLMAdapter debe soportar DOS instancias simultáneas:

```python
class HybridLLMAdapter:
    local: LLMAdapter   # OllamaAdapter — costo $0
    cloud: LLMAdapter   # ClaudeAdapter — costo real

    def invoke(self, task_tier: Literal["LOCAL","CLOUD","AMBIGUOUS"],
               prompt: str, state: ProjectState) -> AdapterResult:
        if task_tier == "LOCAL":
            return self.local.invoke(prompt)
        elif task_tier == "CLOUD":
            return self.cloud.invoke(prompt)
        else:  # AMBIGUOUS
            tier = state.config.ambiguous_tier_preference
            # Configurable per project in .butler.env:
            # AMBIGUOUS_TIER=local|cloud (default: local)
            return getattr(self, tier).invoke(prompt)
```

---

## 4. Visibilidad de Costo para el Usuario

El operador debe poder ver en cualquier momento:
- Tokens Cloud consumidos en la sesión actual
- Tokens Cloud consumidos en el proyecto acumulado
- Estimado de tokens Cloud para completar el milestone actual

Esto es un requisito de UX, no solo de costo — el usuario
necesita poder tomar decisiones informadas sobre cuándo
usar el modelo cloud vs el local.

El Orchestrator muestra un resumen de costo al inicio
de cada sesión y antes de operaciones que
consumen tokens cloud significativos.

---

## 5. Modelo Local Recomendado — Ollama

Para v1.0, el modelo local recomendado es uno de:
- **Qwen2.5-Coder-7B** — mejor para tareas de código
  y estructuras técnicas
- **Llama3.1-8B** — mejor para tareas de lenguaje natural
  y clasificación
- **Mistral-7B-Instruct** — balance entre ambos

El framework no fuerza un modelo específico — cualquier
modelo compatible con Ollama funciona. La recomendación
anterior es orientativa y debe ser revisada en cada
versión del framework dado que el ecosistema evoluciona
rápidamente (stale_after_months: 6).

---

## 6. Métricas de Éxito

- M1: >70% de llamadas a LLM por sesión van a TIER LOCAL.
- M2: El costo Cloud estimado por ciclo completo de
  planeación (Business→Technical, un proyecto mediano)
  es <$2 USD con Claude Sonnet 4.6 a precios actuales.
- M3: El operador tiene visibilidad de costo en tiempo
  real — 0% de sorpresas al final de una sesión larga.
- M4: La calidad de output CLOUD no es degradada por
  tareas que deberían ser CLOUD pero fueron enviadas
  a LOCAL — Agent 9 puede verificar esto via DeepEval.
