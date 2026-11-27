# ADR-014 — SaaS administrado y despliegue institucional

- Fecha: 2026-11-26
- Estado: **In research**
- Base de referencia: `v0.1.0`
- Requisitos: RN-001, RN-002, RN-003, RN-005

## Contexto

El SaaS facilita acceso individual; self-hosted administrado podría interesar a instituciones. No hay validación comercial suficiente.

## Decisión o hipótesis

Comparar SaaS, BYOK e instancia institucional administrada. SaaS es hipótesis inicial, no decisión comercial definitiva. Azure y otros proveedores son alternativas.

## Alternativas consideradas

SaaS centralizado; SaaS BYOK; alojamiento contratado por institución; edición comunitaria futura para usuarios técnicos.

## Consecuencias

Diferentes responsabilidades de mantenimiento, pagos, actualizaciones y soporte. La inferencia sigue siendo cloud en producción.

## Validación pendiente / criterios

Entrevistas, piloto, estimación de soporte y costos, disposición a pagar y requisitos de seguridad. No desarrollar varias ediciones en paralelo.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
