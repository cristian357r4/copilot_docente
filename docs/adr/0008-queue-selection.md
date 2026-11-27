# ADR-008 — Selección de cola y worker

- Fecha: 2026-11-26
- Estado: **Proposed**
- Base de referencia: `v0.1.0`
- Requisitos: RF-003, RF-004, RF-005, RNF-005, RNF-006, RNF-011

## Contexto

Se requieren trabajos persistentes e idempotentes con mínima complejidad operativa sobre el stack Node.js.

## Decisión o hipótesis

Primera PoC candidata: PostgreSQL + pg-boss + worker independiente. No adoptar dependencia definitiva hasta comparar opciones y verificar compatibilidad.

## Alternativas consideradas

Graphile Worker; BullMQ (verificar backend y versión disponibles); Temporal; mecanismos background del proveedor.

## Consecuencias

Compartir PostgreSQL podría reducir servicios; requiere validar semántica de encolado transaccional, reintentos, concurrencia y recuperación. Los efectos externos siguen sujetos a incertidumbre.

## Validación pendiente / criterios

Comparar compatibilidad de Node/Next, licencias, madurez, operación y pruebas de fallos con mock contable; documentar resultados antes de aceptar.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)
- https://pgboss.io/
- https://worker.graphile.org/docs/job-key
- https://docs.bullmq.io/guide/jobs/deduplication
- https://docs.temporal.io/durable-execution

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
