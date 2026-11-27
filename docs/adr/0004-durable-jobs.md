# ADR-004 — Ejecución durable independiente del navegador

- Fecha: 2026-11-26
- Estado: **Accepted (requisito; diseño propuesto)**
- Base de referencia: `v0.1.0`
- Requisitos: RF-003, RF-004, RF-006, RF-012, RNF-005, RNF-011

## Contexto

Cerrar o recargar una página no debe perder una generación aceptada ni obligar a repetir una inferencia costosa.

## Decisión o hipótesis

El ciclo de vida del trabajo será independiente de la conexión del cliente. Propuesta: API que registra y acepta el trabajo, cola durable y worker independiente, con persistencia de estados y resultados. La tecnología concreta se decide en ADR-008.

## Alternativas consideradas

Cola persistente sobre BD; cola especializada; motor de workflows; ejecución background del proveedor como complemento.

## Consecuencias

Requiere gestionar persistencia, recuperación y autorización. No confiar en promesas ejecutadas tras finalizar una petición HTTP. Encolado y persistencia deben ser atómicos o reconciliables mediante outbox.

## Validación pendiente / criterios

Aceptar trabajo, cerrar navegador, recuperar el mismo identificador y resultado; reiniciar worker y comprobar recuperación; simular fallo entre registro y encolado.

## Referencias

- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)
- https://docs.temporal.io/durable-execution

Los criterios de validación son propuestos; este registro no acredita una implementación ni resultados de pruebas adicionales.
