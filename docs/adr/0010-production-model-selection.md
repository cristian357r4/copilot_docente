# ADR-010 — Selección de modelos y proveedor comercial

- Fecha: 2026-11-26
- Estado: **Pending**
- Base de referencia: `v0.1.0`
- Requisitos: RNF-003, RNF-007, RNF-010

## Contexto

NVIDIA NIM funciona en una prueba manual de la base v0.1.0, pero no se ha seleccionado proveedor comercial ni demostrado calidad pedagógica.

## Decisión o hipótesis

No hay decisión de proveedor. Ejecutar benchmarks con casos reproducibles, revisión docente, validez de esquema, calidad curricular, latencia, fallos y costo por planeación aceptada.

## Alternativas consideradas

APIs cloud comerciales de distintos proveedores; modelos abiertos alojados en cloud. NIM experimental y modelos locales solo para desarrollo según ADR-001/003.

## Consecuencias

Requiere medir tarifas y condiciones de uso vigentes, privacidad, cuotas, disponibilidad y compatibilidad. No asumir que un endpoint gratuito es apto para producción.

## Validación pendiente / criterios

Conjunto versionado de casos, resultados comparables, repetición y revisión docente; registrar costos y criterios antes de seleccionar.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)
- https://www.promptfoo.dev/docs/

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
