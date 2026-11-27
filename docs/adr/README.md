# Registro de decisiones arquitectónicas (ADR)

Fecha: 2026-11-26. Base: `v0.1.0` (`ea09e8c`).

Este índice desglosa el registro de [architecture.md](../architecture.md). Los estados Accepted aplican únicamente a la decisión o requisito expresamente indicado, no a una implementación. Proposed, Pending e In research no autorizan cambios de stack. Superseded y Rejected se reservan para decisiones futuras. Consultar [requirements.md](../requirements.md).

| ADR | Estado | Título |
|---|---|---|
| 001 | Accepted | [Inferencia exclusivamente cloud en producción](0001-cloud-only-production.md) |
| 002 | Accepted (requisito; diseño pendiente) | [Independencia de proveedores y modelos de IA](0002-provider-independence.md) |
| 003 | Accepted | [Mocks y modelos locales en desarrollo](0003-development-inference.md) |
| 004 | Accepted (requisito; diseño propuesto) | [Ejecución durable independiente del navegador](0004-durable-jobs.md) |
| 005 | Accepted (requisito; mecanismo propuesto) | [Prevención de duplicados involuntarios](0005-idempotency.md) |
| 006 | Accepted (requisito; mecanismo propuesto) | [Recuperación de trabajos y resultados](0006-job-recovery.md) |
| 007 | Accepted (requisito; diseño propuesto) | [Telemetría y costos por intento](0007-inference-telemetry.md) |
| 008 | Proposed | [Selección de cola y worker](0008-queue-selection.md) |
| 009 | Proposed | [Workflows deterministas antes de agentes autónomos](0009-workflow-strategy.md) |
| 010 | Pending | [Selección de modelos y proveedor comercial](0010-production-model-selection.md) |
| 011 | Accepted (alcance y estrategia inicial; derechos de reutilización pendientes) | [Catálogo curricular estructurado para quinto de primaria](0011-curriculum-knowledge.md) |
| 012 | Proposed | [Separar políticas de facturación de lógica pedagógica](0012-billing-policy.md) |
| 013 | Proposed | [Propiedad de credenciales de inferencia (BYOK)](0013-credential-ownership.md) |
| 014 | In research | [SaaS administrado y despliegue institucional](0014-deployment-business-model.md) |
| 015 | Proposed | [Presupuestos y control de consumo](0015-consumption-budget.md) |

Para modificar una decisión, actualizar su estado, documentar evidencia y mantener sincronizado el índice de architecture.md mediante PR.
