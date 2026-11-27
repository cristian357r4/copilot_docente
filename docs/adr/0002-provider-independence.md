# ADR-002 — Independencia de proveedores y modelos de IA

- Fecha: 2026-11-26
- Estado: **Accepted (requisito; diseño pendiente)**
- Base de referencia: `v0.1.0`
- Requisitos: RNF-003, RNF-010

## Contexto

La base v0.1.0 acopla la generación a integraciones específicas. Necesitamos comparar modelos y sustituirlos sin modificar la lógica pedagógica.

## Decisión o hipótesis

Separar la lógica de negocio de la inferencia mediante un contrato estable. Gateway, registro de modelos y adaptadores son candidatos; no imponer una librería antes de evaluarla.

## Alternativas consideradas

Adaptadores propios; Vercel AI SDK; LiteLLM; acceso directo mediante API compatible, considerando diferencias de capacidades.

## Consecuencias

Permite pruebas y sustitución de proveedores; requiere normalizar errores, capacidades, respuestas y métricas sin perder detalles útiles.

## Validación pendiente / criterios

Probar el mismo caso con mock y NVIDIA sin modificar el servicio de planeación; verificar allowlist de modelos por entorno.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)
- https://ai-sdk.dev/docs
- https://github.com/BerriAI/litellm

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
