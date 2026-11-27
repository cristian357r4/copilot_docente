# ADR-005 — Prevención de duplicados involuntarios

- Fecha: 2026-11-26
- Estado: **Accepted (requisito; mecanismo propuesto)**
- Base de referencia: `v0.1.0`
- Requisitos: RF-005, RF-012, RNF-006, RNF-011

## Contexto

Doble clic, pérdida de respuesta HTTP y peticiones concurrentes pueden crear trabajos duplicados y costos extra. El hash fue una hipótesis, no un requisito.

## Decisión o hipótesis

Garantizar que una misma intención no cree múltiples trabajos; permitir una nueva generación deliberada. Evaluar clave de idempotencia persistida por usuario/operación, restricción única y deduplicación de la cola. Comparar payload normalizado y devolver conflicto al reutilizar una clave con otro contenido. Hash opcional.

## Alternativas consideradas

Bloqueo de botón como complemento; clave de idempotencia; hash; deduplicación de cola; caché (no equivalente a idempotencia).

## Consecuencias

Se deben definir alcance, retención y concurrencia. No deduplicar entre usuarios ni impedir alternativas deliberadas. No garantiza facturación única del proveedor.

## Validación pendiente / criterios

Doble clic, concurrencia, reenvío tras pérdida de respuesta, misma clave con otro payload, nueva intención equivalente y aislamiento de usuarios.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)
- https://docs.stripe.com/api/idempotent_requests
- https://docs.bullmq.io/guide/jobs/deduplication

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
