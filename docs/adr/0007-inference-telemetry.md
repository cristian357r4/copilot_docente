# ADR-007 — Telemetría y costos por intento

- Fecha: 2026-11-26
- Estado: **Accepted (requisito; diseño propuesto)**
- Base de referencia: `v0.1.0`
- Requisitos: RNF-007, RNF-011, RF-012

## Contexto

Necesitamos medir calidad y costos, especialmente en reintentos o llamadas externas cuyo resultado es incierto.

## Decisión o hipótesis

Registrar modelo, proveedor, trabajo, intento, latencia, estado, tokens cuando existan, versiones de configuración y costo estimado o desconocido. Reconciliar resultados inciertos antes de repetir cuando sea posible.

## Alternativas consideradas

Logs estructurados, métricas y persistencia de intentos; OpenTelemetry como candidato posterior.

## Consecuencias

Permite medir costo por planeación aceptada y comparar modelos. No registrar secretos ni datos sensibles; costo desconocido no es cero.

## Validación pendiente / criterios

Verificar métricas de éxito, timeout, error y reintento con mock; comparar tokens reportados por proveedor sin duplicar el registro interno.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
