# ADR-013 — Propiedad de credenciales de inferencia (BYOK)

- Fecha: 2026-11-26
- Estado: **Proposed**
- Base de referencia: `v0.1.0`
- Requisitos: RN-003, RN-004, RNF-008

## Contexto

Trasladar el pago de tokens al cliente puede reducir riesgo financiero, pero aumentar fricción y soporte.

## Decisión o hipótesis

Evaluar credenciales del operador, usuario o institución. BYOK es opcional y no aprobado como funcionalidad del MVP. Nunca exponer claves al frontend o logs.

## Alternativas consideradas

Credenciales gestionadas por operador; BYOK manual; autorización delegada cuando el proveedor la soporte; credencial institucional.

## Consecuencias

Requiere evaluar revocación, cifrado, aislamiento, límites y experiencia de recarga; el cliente no debe instalar modelos.

## Validación pendiente / criterios

Validar disposición del cliente, seguridad, costos de soporte y condiciones del proveedor antes de implementar.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
