# ADR-012 — Separar políticas de facturación de lógica pedagógica

- Fecha: 2026-11-26
- Estado: **Proposed**
- Base de referencia: `v0.1.0`
- Requisitos: RN-003, RN-004

## Contexto

La modalidad comercial no está decidida y el mismo trabajo puede tener distintas fuentes de financiamiento.

## Decisión o hipótesis

Evaluar una política de autorización y facturación independiente de planeaciones e inferencia. No implementar pagos todavía.

## Alternativas consideradas

Suscripción administrada; créditos prepagados; BYOK; facturación institucional.

## Consecuencias

Permite cambiar modalidades con menor acoplamiento; añade requisitos de auditoría y conciliación si se implementa.

## Validación pendiente / criterios

Definir escenarios de cobro y autorización con costos simulados; contrastar con resultados del piloto comercial.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
