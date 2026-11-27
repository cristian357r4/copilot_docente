# ADR-009 — Workflows deterministas antes de agentes autónomos

- Fecha: 2026-11-26
- Estado: **Proposed**
- Base de referencia: `v0.1.0`
- Requisitos: RF-009, RF-011

## Contexto

La automatización integral puede requerir varios pasos, pero una arquitectura multiagente prematura aumenta complejidad y costos.

## Decisión o hipótesis

Evaluar primero workflows explícitos y deterministas. Incorporar autonomía, herramientas y orquestadores especializados solo donde haya beneficio medible.

## Alternativas consideradas

Flujo determinista; agente único con herramientas; multiagente; ejecución durable con Temporal si se justifica.

## Consecuencias

Facilita reproducibilidad, control de costos y depuración; limita flexibilidad hasta evaluar tareas que requieran autonomía.

## Validación pendiente / criterios

Comparar calidad, latencia, costo y revisión humana frente a un flujo simple en casos reales; no seleccionar framework aún.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)
- https://docs.temporal.io/durable-execution

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
