# ADR-015 — Presupuestos y control de consumo

- Fecha: 2026-11-26
- Estado: **Proposed**
- Base de referencia: `v0.1.0`
- Requisitos: RN-004, RNF-007, RNF-011

## Contexto

La inferencia cuesta dinero y pueden existir reintentos, fallos y cargos externos inciertos.

## Decisión o hipótesis

Evaluar cuotas, presupuestos y, si se adopta prepago, reserva y liquidación vinculadas a trabajo e intento. No prometer cobro externo exactamente una vez.

## Alternativas consideradas

Cuotas por operación; créditos; límites monetarios; pago directo por cliente mediante BYOK.

## Consecuencias

Limita exposición financiera; exige política explícita de devoluciones, concurrencia y cargos inciertos. No fijar precios ni límites sin evidencia.

## Validación pendiente / criterios

Simular reservas concurrentes, repetición de peticiones, fallo y conciliación; validar costo real y margen antes de comercializar.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
