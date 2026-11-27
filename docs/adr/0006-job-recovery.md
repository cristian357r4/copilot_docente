# ADR-006 — Recuperación de trabajos y resultados

- Fecha: 2026-11-26
- Estado: **Accepted (requisito; mecanismo propuesto)**
- Base de referencia: `v0.1.0`
- Requisitos: RF-003, RF-004, RF-006, RF-012, RNF-005, RNF-008

## Contexto

Una desconexión no informa necesariamente del estado real del servidor. El docente debe poder recuperar su trabajo.

## Decisión o hipótesis

Persistir el estado y permitir recuperación autorizada tras recarga, cierre, nueva sesión o cambio de dispositivo. Propuesta: API de consulta y listado; polling inicial, SSE si aporta valor. Cancelación explícita, no por cierre de página.

## Alternativas consideradas

Polling; SSE; WebSockets. Los tres son mecanismos de seguimiento, no de durabilidad.

## Consecuencias

Backend como fuente de verdad; requiere autorización y una política de retención. El estado incierto debe diferenciarse del fallo definitivo.

## Validación pendiente / criterios

Probar cierre, pérdida de conexión, recuperación desde otra sesión y acceso denegado a trabajos ajenos; recuperar el resultado sin una segunda inferencia.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
