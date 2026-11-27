# ADR-003 — Mocks y modelos locales en desarrollo

- Fecha: 2026-11-26
- Estado: **Accepted**
- Base de referencia: `v0.1.0`
- Requisitos: RNF-002, RNF-004

## Contexto

Las pruebas de software no deberían depender de tokens de pago ni de disponibilidad externa.

## Decisión o hipótesis

Permitir mocks explícitos, modelos locales y APIs experimentales durante desarrollo. En producción solo proveedores cloud autorizados. Nunca presentar una respuesta mock como inferencia real.

## Alternativas consideradas

Mock fixtures para CI; Ollama o LM Studio para experimentación; NVIDIA NIM y APIs comerciales para benchmarks.

## Consecuencias

Reduce consumo y permite pruebas reproducibles; los resultados locales no garantizan calidad de producción.

## Validación pendiente / criterios

Ejecutar pruebas convencionales sin claves ni acceso de red; identificar claramente respuestas simuladas; impedir proveedores exclusivos de desarrollo en producción.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)
- https://docs.ollama.com/
- https://lmstudio.ai/docs

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
