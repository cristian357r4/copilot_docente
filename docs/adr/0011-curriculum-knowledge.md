# ADR-011 — Catálogo curricular estructurado para quinto de primaria

- Fecha: 2026-11-26
- Estado: **Accepted (alcance y estrategia inicial; derechos de reutilización pendientes)**
- Base de referencia: `v0.1.0`
- Requisitos: RF-009, RNF-010

## Contexto

La generación actual depende principalmente de instrucciones proporcionadas al modelo y no cuenta con un catálogo curricular estructurado que permita validar las referencias utilizadas en las planeaciones.

Se requiere incorporar referencias curriculares oficiales de forma verificable, manteniendo un alcance reducido durante el MVP.

## Decisión

Para el MVP de Copilot Docente se establece el siguiente alcance:

1. Nivel educativo: educación primaria en México.
2. Grado: exclusivamente quinto de primaria.
3. Referencia curricular inicial: Programa de Estudio para la Educación Primaria, Programa Sintético de la Fase 5, SEP, primera edición 2025.
4. Campos formativos: Lenguajes; Saberes y Pensamiento Científico; Ética, Naturaleza y Sociedades; De lo Humano y lo Comunitario.
5. Incorporar únicamente los contenidos y Procesos de Desarrollo de Aprendizaje correspondientes a quinto grado.

La estrategia técnica inicial será un catálogo estructurado y versionado, consultado mediante filtros y relaciones explícitas entre grado, campo formativo, contenido y PDA.

No se incorporará RAG, recuperación vectorial, embeddings ni una base de datos vectorial como requisito del MVP.

El mecanismo físico de almacenamiento del catálogo se mantiene independiente de esta decisión y podrá comenzar mediante archivos de datos versionados.

## Principios de uso curricular

- Mantener la referencia a la fuente oficial, edición y página.
- No mezclar PDA de quinto y sexto grado.
- No inventar relaciones curriculares que no estén respaldadas por la fuente.
- Distinguir las referencias oficiales de las sugerencias pedagógicas generadas por IA.
- Permitir que el docente contextualice, adapte y revise las propuestas.
- No presentar la planeación generada como sustituto del Programa Analítico de la escuela.
- No imponer una secuencia curricular oficial que la fuente no establezca.

## Derechos de uso de las fuentes

El Programa Sintético de la Fase 5, edición 2025, establece condiciones de reproducción para fines no comerciales.

Por tanto, la incorporación o reproducción de textos oficiales en un producto comercial queda pendiente de revisión jurídica y, cuando corresponda, autorización de sus titulares.

La disponibilidad pública de un documento no implica autorización automática para su reproducción comercial.

## Alternativas consideradas

- Catálogo curricular estructurado.
- RAG sobre documentos oficiales.
- Búsqueda híbrida y recuperación vectorial.
- Incorporación simultánea de todas las fases de primaria.

Se selecciona el catálogo estructurado y el alcance de quinto grado para reducir complejidad, facilitar la validación y limitar el trabajo inicial de mantenimiento.

RAG y la ampliación curricular permanecen como alternativas futuras, sujetas a evidencia de necesidad.

## Consecuencias

- Menor complejidad de infraestructura.
- No se requieren embeddings ni servicios vectoriales.
- Mayor control sobre las referencias curriculares enviadas al modelo.
- Necesidad de mantener y versionar manualmente el catálogo.
- Alcance inicial limitado a quinto de primaria.
- Dependencia de la verificación de fuentes y derechos de reutilización.
- Necesidad de revisión docente para evaluar la pertinencia pedagógica.

## Validación pendiente / criterios

1. Verificar la integridad de los registros de quinto grado en los cuatro campos formativos.
2. Confirmar que no se incorporen PDA exclusivos de sexto grado.
3. Verificar la correspondencia entre contenido y PDA contra el documento oficial.
4. Conservar la trazabilidad de fuente, edición y página.
5. Evaluar las planeaciones con docentes de quinto grado.
6. Comprobar que las referencias curriculares generadas correspondan a registros existentes.
7. Resolver las condiciones de reutilización antes de distribuir contenido protegido en un producto comercial.

Estos criterios se detallan, en el mismo orden, en TST-016 a TST-022 del
[plan de pruebas](../testing/test-plan.md); TST-023 cubre los principios de
presentación y revisión docente. Todos permanecen PENDING: definirlos no
acredita una ejecución. La versión del catálogo debe acompañar la trazabilidad
de fuente, edición y página. La revisión de derechos es un control documental
separado, no un resultado de inferencia ni una autorización automática.

## Referencias

- Programa Sintético de la Fase 5, SEP, primera edición 2025: https://educacionbasica.sep.gob.mx/wp-content/uploads/2025/Plan_y_programas_de_estudio_2025/WEB%20FASE%205-2025.pdf
- https://dgdcpei.sep.gob.mx/Primaria
- [Arquitectura](../architecture.md)
- [Requisitos](../requirements.md)

Este ADR aprueba el alcance curricular y la estrategia técnica inicial. No acredita que el catálogo esté implementado, validado pedagógicamente o autorizado para reproducción comercial.
