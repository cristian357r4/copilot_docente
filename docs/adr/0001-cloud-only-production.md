# ADR-001 — Inferencia exclusivamente cloud en producción

- Fecha: 2026-11-26
- Estado: **Accepted**
- Base de referencia: `v0.1.0`
- Requisitos: RNF-001, RNF-002, RNF-009, RN-002

## Contexto

Los docentes pueden tener equipos básicos; instalar modelos locales reduce el alcance y aumenta la fricción.

## Decisión o hipótesis

La inferencia de producción se ejecutará mediante infraestructura cloud remota. El navegador no ejecutará modelos ni requerirá instalaciones adicionales. No se elige proveedor ni plataforma de alojamiento.

## Alternativas consideradas

Inferencia en el equipo del docente (descartada para producción); API administrada o inferencia remota en cloud (implementación pendiente).

## Consecuencias

Cliente ligero y experiencia uniforme; dependencia de conectividad y proveedor. El alojamiento institucional de la aplicación no modifica esta restricción.

## Validación pendiente / criterios

Validar que producción rechace proveedores locales y que el flujo web funcione sin software adicional.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
