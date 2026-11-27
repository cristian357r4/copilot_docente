# Copilot Docente — Requisitos del producto

Fecha: 2026-11-26. Revisión: 0.1. Base del software: `v0.1.0` (`ea09e8c`). Estado: documento de trabajo.

## 1. Objetivo y alcance

Reducir el tiempo que los docentes mexicanos dedican a preparar planeaciones y realizar tareas repetitivas, mediante una aplicación web accesible desde equipos de prestaciones básicas. El producto debe favorecer resultados editables, verificables y recuperables, con revisión humana.

El segmento inicial de educación básica es una hipótesis de mercado. La modalidad comercial, los precios, los proveedores cloud y el alcance exacto del piloto aún deben validarse con docentes. No confundir la arquitectura objetivo con funcionalidades ya implementadas.

El alcance curricular del MVP ya está acotado por [ADR-011](adr/0011-curriculum-knowledge.md)
a quinto de primaria en México, con catálogo estructurado y versionado; esa
decisión no equivale a validación de mercado ni a implementación. Cubre los
cuatro campos: Lenguajes; Saberes y Pensamiento Científico; Ética, Naturaleza
y Sociedades; y De lo Humano y lo Comunitario. Excluye contenidos y PDA
exclusivos de sexto, RAG, embeddings y bases vectoriales en el MVP. Los derechos
de reutilización permanecen pendientes; su revisión y, cuando corresponda,
autorización deben resolverse antes de distribuir contenido protegido en un
producto comercial.

## 2. Convenciones

- **Acordado:** necesidad o restricción establecida en las conversaciones de diseño; no implica que esté implementada.
- **Propuesto:** funcionalidad o criterio candidato que necesita revisión o validación.
- **Pendiente:** decisión abierta que requiere investigación o evidencia.
- Los criterios de aceptación son propuestas verificables para convertir los requisitos en casos de prueba. No constituyen resultados de pruebas ejecutadas.
- Las prioridades indicadas son provisionales y no son compromisos de entrega.

## 3. Requisitos funcionales

| ID | Requisito | Estado | Criterio de aceptación propuesto |
|---|---|---|---|
| RF-001 | Generar una planeación con los parámetros proporcionados por el docente. | Acordado; base funcional NVIDIA | Con datos válidos, se presenta una planeación con inicio, desarrollo, cierre y evaluación; la cantidad de sesiones coincide con la solicitada. Caso TC-AI-001. |
| RF-002 | Diferenciar una generación exitosa de una fallida. | Acordado; parcialmente implementado | Un proveedor no configurado, una respuesta inválida o un fallo de comunicación no devuelve una planeación predefinida como si fuera real. |
| RF-003 | Continuar un trabajo aceptado aunque el navegador se cierre o se desconecte. | Acordado | Tras aceptar un trabajo, cerrar la pestaña no cancela su ejecución. Al volver, se consulta su estado persistido. |
| RF-004 | Recuperar trabajos activos y resultados anteriores. | Acordado | El usuario autorizado puede recuperar el trabajo desde una nueva sesión o dispositivo, sin volver a ejecutar la generación. |
| RF-005 | Evitar duplicados involuntarios. | Acordado | Un doble clic o reenvío de la misma intención no crea un segundo trabajo. Una nueva generación deliberada sigue siendo posible. |
| RF-006 | Mostrar estados y errores de los trabajos. | Propuesto | Se distingue entre pendiente, en ejecución, completado, fallido y resultado incierto; una desconexión no se presenta como fallo definitivo del trabajo. |
| RF-007 | Permitir revisar y editar la planeación. | Propuesto; prioridad alta | El docente puede modificar el contenido antes de aprobarlo sin solicitar una inferencia completa adicional. |
| RF-008 | Exportar planeaciones a documentos editables y formatos de entrega. | Propuesto; prioridad alta | Un resultado aprobado se exporta a Word; PDF y plantillas institucionales se evalúan según necesidades del piloto. |
| RF-009 | Utilizar referencias curriculares verificables de la SEP, limitadas a quinto de primaria. | Propuesto; prioridad alta | Catálogo estructurado y versionado de los cuatro campos, con fuente oficial, edición y página por referencia. Comprobar correspondencia contenido/PDA y excluir contenidos y PDA exclusivos de sexto. Las referencias usadas deben existir en esa versión del catálogo; distinguirlas de sugerencias generadas, sin inventar alineación ni secuencias oficiales. Validar con docentes de quinto; derechos de reutilización pendientes. Criterios TST-016 a TST-023, todavía PENDING. |
| RF-010 | Conservar contexto docente y continuidad curricular. | Propuesto | Se pueden recuperar parámetros del grupo, planeaciones anteriores y avance curricular autorizado. No se requieren datos identificables de menores para el piloto. |
| RF-011 | Automatizar tareas recurrentes y elaborar materiales complementarios. | Propuesto; posterior al núcleo funcional | Un flujo produce los resultados solicitados y permite revisión docente antes de entregarlos o aprobarlos. |
| RF-012 | Cancelar o reintentar explícitamente trabajos cuando proceda. | Propuesto | Cerrar el navegador no cancela; una cancelación solicitada distingue entre trabajo detenido y petición externa ya iniciada. Un resultado incierto no se reintenta ciegamente. |

## 4. Requisitos no funcionales y restricciones

| ID | Requisito | Estado | Criterio de aceptación propuesto |
|---|---|---|---|
| RNF-001 | Acceso web sin instalación adicional por el docente. | Acordado | Las operaciones principales se realizan desde un navegador compatible, sin instalar modelos ni runtimes de IA. |
| RNF-002 | Inferencia exclusivamente cloud en producción. | Acordado | El backend de producción rechaza modelos y proveedores exclusivos de desarrollo; no realiza inferencia en el equipo del docente. |
| RNF-003 | Independencia respecto de los proveedores y modelos. | Acordado | Se sustituye un modelo compatible sin reescribir el flujo de planeación ni cambiar la interfaz del docente. |
| RNF-004 | Desarrollo con bajo costo y pruebas reproducibles. | Acordado | Las pruebas convencionales pueden ejecutarse con mocks sin credenciales reales; la inferencia local se admite solo para experimentación y desarrollo. |
| RNF-005 | Persistencia y tolerancia a desconexiones. | Acordado | El estado y resultado de un trabajo aceptado no dependen de una conexión HTTP abierta; una nueva sesión autorizada puede recuperarlos. |
| RNF-006 | Control de concurrencia y duplicados. | Acordado | Peticiones concurrentes de la misma intención generan un único trabajo; el mecanismo de idempotencia se seleccionará mediante investigación y PoC. |
| RNF-007 | Medición de consumo y costos. | Acordado | Se registran proveedor, modelo, intento, latencia, estado y tokens cuando estén disponibles; los costos desconocidos se identifican como tales, no como cero. |
| RNF-008 | Seguridad de credenciales y aislamiento de usuarios. | Acordado como restricción de diseño | Los secretos no se versionan ni se envían al frontend o logs; un usuario no puede recuperar trabajos ajenos. Verificar con pruebas antes de un piloto multiusuario. |
| RNF-009 | Cliente ligero y experiencia usable en equipos básicos. | Acordado | No hay procesamiento de IA en el navegador. Definir dispositivos y conexiones de referencia para medir tiempos de carga y usabilidad antes del piloto. |
| RNF-010 | Calidad de las respuestas. | Propuesto | Validación de esquema y revisión de contenido mediante casos reproducibles; evaluación docente de coherencia y alineación curricular. |
| RNF-011 | Reintentos y costos externos controlados. | Acordado como objetivo | Registrar intentos y estados inciertos; no prometer facturación externa exactamente una vez si el proveedor no ofrece esa garantía. |
| RNF-012 | Mantenibilidad y trazabilidad. | Propuesto | Cambios mediante ramas y PR, lockfile versionado, pruebas reproducibles y ADR para decisiones relevantes. |
| RNF-013 | Protección de datos durante experimentación y piloto. | Propuesto | Usar datos sintéticos o no identificables; revisar políticas de proveedores y requisitos legales antes de manejar información personal de alumnos. |

No se han acordado SLA, límites de latencia, throughput, disponibilidad porcentual ni presupuestos numéricos. Se establecerán a partir de mediciones y necesidades de usuarios; no inventar objetivos como si estuvieran aprobados.

## 5. Requisitos de negocio e hipótesis

| ID | Hipótesis / necesidad | Estado |
|---|---|---|
| RN-001 | Reducir trabajo repetitivo y tiempo de corrección del docente. | Objetivo; beneficio por medir. |
| RN-002 | Ofrecer una experiencia sin conocimientos técnicos de IA. | Acordado como principio de producto. |
| RN-003 | SaaS administrado, BYOK opcional y despliegue institucional. | Alternativas por validar; ninguna modalidad definitiva. |
| RN-004 | Limitar exposición financiera mediante políticas de consumo. | Propuesto; evaluar cuotas, créditos y facturación directa al proveedor. |
| RN-005 | Competir mediante automatización integral, no solo generación de texto. | Hipótesis de diferenciación; contrastar con competidores y entrevistas. |

No implementar pagos, gestión de claves de clientes o ediciones múltiples únicamente porque figuran como hipótesis. Azure es una alternativa de infraestructura, no un requisito ni una decisión aprobada.

## 6. Alcance incremental propuesto

- **Base funcional:** generación NVIDIA, correcciones de frontend/backend y manejo explícito de errores (v0.1.0, validación manual limitada).
- **Núcleo técnico:** abstracción de IA, mock, validación, telemetría y evaluación de soluciones existentes para trabajos durables.
- **Piloto docente:** planeación editable, exportación Word, conocimiento curricular verificable y recuperación de resultados. El orden detallado se ajustará con la investigación y la prueba de concepto.
- **Evolución:** perfiles, contexto persistente, automatización recurrente, agentes y modalidades comerciales si aportan valor comprobable.

Fuera del alcance inicial propuesto: marketplace, juegos interactivos, sistema escolar completo, inferencia en equipos docentes, infraestructura GPU propia en producción y desarrollo simultáneo de varias ediciones comerciales.

## 7. Trazabilidad con arquitectura

| Requisitos | ADR relacionados en architecture.md |
|---|---|
| RNF-001, RNF-002, RNF-004 | 001, 003 |
| RNF-003, RNF-010 | 002, 010, 011 |
| RF-003, RF-004, RF-006, RNF-005 | 004, 006, 008 |
| RF-005, RF-012, RNF-006, RNF-011 | 005, 004, 007, 008 |
| RNF-007 | 007, 010, 015 |
| RF-009, RF-011 | 009, 011 |
| RN-003, RN-004 | 012, 013, 014, 015 |

Para RF-009/RNF-010, ADR-011 define el alcance y la estrategia; TST-016 a
TST-023 detallan su validación pendiente en [test-plan.md](testing/test-plan.md).
RF-011 y ADR-009 no autorizan ampliar el alcance curricular del MVP.

La relación es orientativa. Los ADR propuestos no establecen dependencias tecnológicas obligatorias. Consultar [architecture.md](architecture.md) para alternativas, restricciones y estado de decisiones.

## 8. Preguntas abiertas

1. ¿Qué formatos, tareas y frecuencia de trabajo generan mayor carga para docentes reales?
2. ¿Qué criterios curriculares y documentales consideran imprescindibles los usuarios piloto?
3. ¿Qué nivel de calidad, latencia y costo por planeación aceptada resulta viable?
4. ¿Qué solución existente satisface la persistencia, concurrencia e idempotencia con menor complejidad?
5. ¿Qué modalidad de monetización ofrece menor fricción y suficiente margen, considerando soporte?
6. ¿Qué requisitos de privacidad, retención y seguridad aplican antes de manejar datos reales?

## 9. Evidencia y limitaciones

La base v0.1.0 fue compilada y probada manualmente mediante NVIDIA con TC-AI-001; OpenAI, el DNS intermitente, validación completa del esquema y calidad pedagógica siguen pendientes. Este documento se deriva de las decisiones y propuestas recogidas en la conversación y en architecture.md. No proviene de entrevistas formales, benchmarks o auditoría de mercado; debe revisarse al obtener esa evidencia.

TC-AI-001 identifica el escenario funcional de referencia de la sección 3.2
del [plan de pruebas](testing/test-plan.md); TST-005 registra su ejecución
histórica con NVIDIA. Su PASS funcional no valida la correspondencia curricular
del tema de fracciones, ni el catálogo o sus derechos. No se reejecutó el caso
durante la sincronización documental de ADR-011.
