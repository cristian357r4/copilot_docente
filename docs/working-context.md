# Copilot Docente — Plan de continuidad

Fecha del diagnóstico: **2026-11-26**, zona `America/Mexico_City`.
Repositorio local: `C:\Users\xazak\Documents\AgentesEducation\copilot_docente`.
Rama verificada: `docs/architecture`.
Línea base que debe conservarse: **v0.1.0**, commit
`ea09e8c27de7f7cd90c2971e091ce82eb42f8efa`.

## 1. Objetivo y límites de trabajo

Exportar el contexto y el trabajo pendiente para continuarlo desde VS Code.
La fase actual es consolidación documental e investigación; no es una fase
de implementación de funcionalidades.

**Etapa 0 realizada:** diagnóstico local, lectura de instrucciones y documentos,
y creación de este plan. No existía un `working-context.md` previo.
**Etapa 1 autorizada y completada documentalmente:** el usuario aprobó continuar
tras recibir el diagnóstico. Se sincronizaron ADR-011 y su trazabilidad; ver
el registro de validación de la sección 5.2. Catálogo y derechos siguen pendientes.
**Punto de parada actual:** presentar resultados y esperar autorización del
usuario antes de iniciar la etapa 2 u otros cambios o decisiones posteriores.
La autorización de una etapa no autoriza automáticamente las siguientes.

**Excepción autorizada el 2026-11-26:** el usuario solicitó hacer, esta ocasión,
un primer commit local con los 23 documentos existentes (`AGENTS.md` y los
22 Markdown de `docs/`). La autorización incluye su staging explícito y este
registro; no incluye push, PR ni avanzar a la etapa 2. El commit conserva
`v0.1.0` en `ea09e8c` como línea base funcional; HEAD avanzará solo por documentación.

Reglas que permanecen vigentes:

- Conservar los cambios locales del usuario y trabajar en la rama actual.
- No cambiar código funcional, stack, dependencias, proveedores ni modelos.
- No implementar catálogo, gateway, persistencia, colas, workers o pagos.
- No leer ni imprimir secretos; no abrir archivos reales de credenciales.
- No hacer staging ni commits adicionales fuera del primer commit documental
  autorizado arriba; no hacer push, PR, merge ni alterar etiquetas.
- Commit/PR, configuración de Codex Cloud y creación de GitHub Issues requieren
  autorización explícita posterior y revisión de los resultados documentales.
- Una decisión aceptada no acredita implementación; una propuesta no autoriza
  instalar dependencias ni marcar ADR como Accepted.

## 2. Evidencia y estados

- **Verificado:** observado directamente en archivos o comandos locales durante
  este diagnóstico. Cuando se verifica una afirmación documental, eso acredita
  que está escrita, no que la funcionalidad haya sido ejecutada.
- **No verificado:** antecedente histórico, comportamiento o dato externo que
  necesita evidencia adicional. No equivale a fallo ni a ausencia de código.
- **Propuesto:** trabajo, criterio o alternativa pendiente de aprobación y/o
  validación. No constituye una decisión de tecnología.

Se consultó la conversación «Buscar proyectos educativos en GitHub», ID
`6b0697d2-6f08-83e8-acb0-a6aceabce6c7`, como antecedente. Las comprobaciones
locales siguientes prevalecen sobre los resúmenes históricos de su estado.

### 2.1 Verificado localmente en el diagnóstico inicial (etapa 0)

| Elemento | Resultado y evidencia |
|---|---|
| Acceso y rama | Repositorio accesible; `git branch --show-current`: `docs/architecture`. |
| Línea base | `git rev-parse HEAD` y `git rev-parse 'v0.1.0^{commit}'` coinciden en el commit indicado arriba. |
| Árbol inicial | 22 documentos locales nuevos respecto de HEAD: cinco con intent-to-add y 17 sin seguimiento. Ningún cambio de código funcional aparece en `git status`. |
| Índice | Los cinco archivos con ` A` son ADR-011, índice ADR, arquitectura, requisitos y roadmap. `git ls-files --debug` muestra flags `20004000`; `git diff --cached --stat` está vacío. No confundir intent-to-add con contenido preparado para commit. |
| Instrucciones | Leído `AGENTS.md`; no se encontraron instrucciones adicionales en los ancestros consultados ni un `AGENTS.md` más específico en el repositorio. |
| Lecturas | Leídos `README.md`, `package.json`, arquitectura, requisitos, roadmap, deuda técnica, índice ADR, los 15 ADR individuales y plan de pruebas. |
| Stack declarado localmente | `package.json`: Next 13.4.10, React 18.2.0, JavaScript, Axios 1.5.0 y OpenAI SDK 4.11.1. Scripts: dev, build, start, lint; no declara script test. No se auditaron dependencias instaladas. |
| ADR-011 | Aceptado solo en alcance y estrategia inicial; quinto de primaria México, catálogo estructurado versionado, sin RAG/embeddings/base vectorial en el MVP; derechos pendientes. |
| Sincronización parcial | Índice y arquitectura reflejan ADR-011; RF-009 ya limita quinto y permanece Propuesto; M2/M3 y exclusiones del roadmap ya incluyen catálogo y límites del MVP. |
| Investigación persistida | `docs/research/` existe y no contiene archivos al diagnóstico. La investigación previa de proveedores de inferencia aún necesita recuperación y trazabilidad local. |
| Plan anterior | `docs/working-context.md` no existía; este archivo fue la única incorporación de la etapa 0. |

### 2.2 No verificado en esta sesión

- No se reejecutaron build, servidor, inferencias, benchmarks ni pruebas
  funcionales. Los PASS de TST-001 a TST-005 son evidencia histórica registrada.
- NVIDIA NIM y `nvidia/nemotron-3.5-lightning-30b-a3b` son la referencia de
  prueba documentada, no una selección comercial ni garantía de disponibilidad.
- OpenAI de extremo a extremo sigue pendiente en TST-007; Gemini y Anthropic
  son candidatos de evaluación, sin integración ni aprobación acreditadas.
- No se inspeccionó nuevamente el código funcional: catálogo, persistencia,
  idempotencia, autenticación y telemetría no se dan por implementados.
- Eliminar el fallback silencioso es una afirmación de arquitectura; TD-004
  y TST-011 conservan observaciones anteriores por verificar contra el código.
- No se verificaron causa raíz DNS, rotación de credenciales, ausencia de
  secretos en historial, seguridad, privacidad ni calidad pedagógica general.
- No se validaron la edición SEP, integridad de contenidos, condiciones de
  reutilización, versiones actuales de herramientas, precios ni límites cloud.
  La edición 2025 y sus condiciones son referencias registradas en ADR-011,
  no una conclusión jurídica nueva de este diagnóstico.
- No se comprobó sincronización con remotos ni se hizo fetch; la coincidencia
  de HEAD y etiqueta es una comprobación local.

### 2.3 Estado documental de los ADR

| ADR | Estado leído; no acredita implementación |
|---|---|
| 001, 003 | Accepted: cloud en producción; mocks/local en desarrollo. |
| 002 | Accepted como requisito; diseño de independencia pendiente. |
| 004, 005, 006, 007 | Accepted como requisitos; diseño o mecanismo propuesto. |
| 008, 009 | Proposed: cola/worker y estrategia de workflows. |
| 010 | Pending: modelos y proveedor comercial sujetos a benchmark. |
| 011 | Accepted: alcance y estrategia inicial; reutilización pendiente. |
| 012, 013, 015 | Proposed: facturación, BYOK y presupuestos. |
| 014 | In research: modalidades de alojamiento y negocio. |

## 3. Hallazgos iniciales y estado de seguimiento

| ID | Hallazgo verificado en documentación | Acción propuesta |
|---|---|---|
| H-01 | En el diagnóstico inicial, pruebas y AGENTS no explicitaban todos los límites de ADR-011; TD-007 aún planteaba decidir entre catálogo y RAG. | Resuelto documentalmente en etapa 1: AGENTS 3.4, TD-007 y TST-016 a TST-023. Catálogo, revisión docente y derechos siguen pendientes; TD-007 permanece abierto. |
| H-02 | En el diagnóstico inicial, arquitectura/requisitos citaban TC-AI-001 y el plan usaba TST-005 sin equivalencia explícita. | Resuelto documentalmente en etapa 1: TC-AI-001 es el escenario de la sección 3.2; TST-005 es su ejecución histórica con NVIDIA. Se conservan los estados y evidencias previos. |
| H-03 | M3 exige conservar/recuperar trabajo; M4 contiene persistencia y seguridad y depende de requisitos de M1/M3. | Aclarar qué capacidades mínimas de M4 preceden a los criterios de salida de M3 y al piloto; evitar una secuencia circular de implementación. |
| H-04 | Arquitectura declara retirado el fallback; TD-004/TST-011 conservan el hallazgo histórico por verificar. README todavía describe fallback y modelos antiguos. | Separar relato histórico de estado comprobado; proponer revisión de código sin modificarlo en esta fase. |
| H-05 | TD-009 menciona hash de entrada entre las acciones; ADR-005 y arquitectura lo consideran opcional. | Alinear el lenguaje: clave por intención/usuario, comparación de payload y hash opcional. |
| H-06 | Arquitectura contiene afirmaciones de compatibilidad, incluidas versiones/backends de BullMQ, sin comprobación nueva. | Verificar versiones y fuentes primarias en la investigación; mantenerlas como hipótesis. |
| H-07 | No hay artefactos en research que respalden la investigación previa de inferencia. | Recuperar fuentes y fechas antes de considerar esa investigación consolidada. |

## 4. Plan por etapas y autorizaciones

### Etapa 1 — Completar sincronización de ADR-011

**Estado:** autorizada por el usuario y completada documentalmente el 2026-11-26.
Se conservaron las partes ya correctas y se añadieron los criterios faltantes.
La validación funcional del catálogo y la revisión de derechos no se ejecutaron.

Archivos a revisar con cambios mínimos: [índice ADR](adr/README.md),
[ADR-011](adr/0011-curriculum-knowledge.md), [arquitectura](architecture.md),
[requisitos](requirements.md), [roadmap](roadmap.md),
[pruebas](testing/test-plan.md), [deuda técnica](technical-debt.md) y
`../AGENTS.md`.

1. Conservar los cambios que ya sincronizan índice, arquitectura, RF-009 y
   roadmap. Añadir solo aclaraciones faltantes; RF-009 sigue Propuesto.
2. Hacer explícitos quinto de primaria en México, cuatro campos formativos,
   catálogo estructurado y versionado, fuente/edición/página y derechos
   pendientes. Separar contenidos y PDA de quinto de los de sexto.
3. Mantener fuera del MVP RAG, embeddings y bases vectoriales. El almacenamiento
   físico no está decidido; los archivos versionados son una posibilidad.
4. Actualizar TD-007 y las instrucciones curriculares de AGENTS para evitar
   que otro agente reabra una decisión ya aceptada por omisión documental.
5. Preparar casos PENDING: integridad de los cuatro campos, exclusión de PDA
   exclusivos de sexto, correspondencia contenido/PDA, referencias existentes,
   fuente/edición/página/versión y revisión docente. No inventar registros ni
   marcar PASS sin ejecución. La revisión de derechos es una condición separada.
6. Aclarar trazabilidad TC-AI-001/TST-005 y mantener intactas evidencias históricas.

**Salida obtenida:** matriz documento → cláusula ADR-011 → requisito → prueba
propuesta en la sección 5.1 del [plan de pruebas](testing/test-plan.md).
No se detectaron contradicciones curriculares en los documentos revisados;
catálogo y derechos siguen pendientes. La etapa 2 no está autorizada todavía.

### Etapa 2 — Revisar ADR-008 y preparar comparativa de trabajos durables

**Estado:** propuesta, ninguna cola seleccionada ni PoC autorizada.

Entregable propuesto: `docs/research/queue-workers-comparison.md`, enlazado
desde [ADR-008](adr/0008-queue-selection.md) y arquitectura cuando corresponda.

Comparar **pg-boss, Graphile Worker, BullMQ, Temporal y colas cloud
administradas**. Identificar servicios cloud concretos durante la investigación,
sin convertirlos de antemano en requisitos. Separar colas, motores de workflows
y capacidades background del proveedor de inferencia.

Para cada opción documentar:

- Versión, licencia, madurez, mantenimiento y compatibilidad Node/Next del
  proyecto; worker independiente y límites de ejecución del alojamiento.
- Infraestructura requerida, costo fijo y variable, operación, backups,
  disponibilidad, egreso y supuestos de carga. No inventar precios ni usar
  créditos gratuitos como costo de operación permanente.
- Registro y encolado atómicos o transactional outbox; concurrencia, bloqueos,
  leases, reentrega, reintentos, recuperación, orden y trabajos fallidos.
- Idempotencia de intención por usuario, payload conflictivo, retención de
  claves, nueva generación deliberada y diferencia entre deduplicar una cola y
  evitar efectos externos repetidos.
- Límite de garantías: no prometer exactly-once en llamadas o cobros externos.
  Tratar timeout tras envío como resultado potencialmente incierto; reconciliar
  cuando el proveedor lo permita antes de reintentar.

Diseñar, sin ejecutar ni instalar aún, una PoC con mock contable: doble clic,
concurrencia, respuesta HTTP perdida, reinicio de worker, fallo entre persistir
y encolar, desconexión, misma clave con payload distinto, autorización cruzada
y timeout incierto. Registrar qué mide cada prueba y qué no demuestra.

**Salida:** comparación con fuentes y vacíos explícitos; ADR-008 continúa
Proposed. pg-boss + PostgreSQL es candidato inicial, no compromiso de stack.
Presentar opciones y esperar autorización antes de PoC o decisión.

### Etapa 3 — Alojamiento cloud, modelos concretos y SDK/adaptadores

**Estado:** propuesta; la investigación anterior de inferencia es antecedente,
su respaldo local está pendiente. No repetirla sin comprobar qué puede recuperarse.

Entregables propuestos en `docs/research/`:

1. `cloud-hosting-comparison.md`: comparar alternativas de alojamiento para
   Next/API, worker persistente, almacenamiento y cola. Revisar procesos largos,
   escalado, conexiones, regiones, secretos, logs, backups/restauración, despliegue
   y costo total bajo supuestos explícitos. Azure es candidato, no requisito.
2. `models-and-adapters-comparison.md`: recuperar investigación previa y evaluar
   modelos con identificador/versión concretos, disponibilidad, esquema/JSON,
   límites, tokens, latencia, cuotas, privacidad y costo por planeación aceptada.
   NVIDIA permanece como referencia de prueba; OpenAI, Gemini y Anthropic son
   candidatos sujetos a benchmark, sin aprobación comercial ni garantía E2E.
3. Comparar SDK oficiales, adaptadores propios y candidatos ya citados en
   ADR-002, como Vercel AI SDK y LiteLLM. Evaluar compatibilidad real con el
   stack, contrato común, validación, errores, consumo, cancelación, capacidades
   específicas, mantenimiento y costo operativo. No adoptar librerías todavía.

Mantener independientes la elección de alojamiento, cola, modelo/proveedor de
inferencia y SDK. Poder alojar la aplicación en una nube no obliga a contratar
su inferencia. No comparar solo nombres de proveedores ni elegir por una demo.

Cada investigación debe registrar fecha de consulta, URL primaria, versión o
fecha de tarifa, moneda/región, unidad de cobro, escenario de carga, límites,
inferencia propia y datos no verificados. Si falla el acceso, registrar el fallo
y dejar pendiente la conclusión; no completar datos de memoria como vigentes.

**Salida:** fuentes recuperables y plan de benchmark común con casos, rúbrica,
configuración y revisión docente. ADR-010 sigue Pending hasta evaluación y
autorización. No ejecutar inferencias facturables en esta fase.

### Etapa 4 — Consolidar auditoría documental previa

**Estado:** propuesta; no es una auditoría funcional ni de secretos ya realizada.

- **Identidad/autorización/privacidad:** RNF-008/013, TD-012, ADR-004/006/013;
  acceso solo a trabajos propios, aislamiento, minimización, retención,
  eliminación y políticas de proveedores. Revisar requisitos y casos faltantes
  antes de un piloto, sin seleccionar autenticación por anticipado.
- **Estados:** alinear RF-006/012 con QUEUED, PROCESSING, VALIDATING, COMPLETED,
  FAILED, CANCELLED y OUTCOME_UNKNOWN propuestos; definir transiciones,
  persistencia, cancelación, reconciliación y límites de reintento. No suponer
  que estos estados o endpoints ya existen.
- **M3/M4:** aclarar dependencias mínimas de persistencia, identidad y seguridad
  para conservar resultados y operar un piloto; separar diseño de implementación.
- **DNS:** mantener TD-002/TST-006 como intermitente; los éxitos posteriores
  no cierran causa raíz. Preparar evidencia sanitizada y condiciones reproducibles.
- **Credenciales:** mantener TD-001/TST-015 con sus límites históricos; rotación
  a cargo del propietario, sin leer valores, revocar ni escanear historial en
  esta etapa. Cualquier auditoría posterior debe redactar resultados sensibles.
- **Fallback y validación:** conciliar H-04, TD-004/005 y TST-008/011. Revisión
  futura del código solo de lectura y dentro del alcance aprobado; no cerrar
  hallazgos por una afirmación documental ni presentar HTTP 200 como calidad.
- **Telemetría:** ADR-007/015, TD-011 y RNF-007/011; distinguir trabajo, intento,
  tokens reportados, costo estimado/desconocido, cargo externo incierto y cargo
  interno. Mantener hash opcional según ADR-005 y corregir H-05.

**Salida:** hallazgo → evidencia → documento/ADR/requisito → caso pendiente →
criterio de cierre. Conservar historia y no cambiar estados sin evidencia.
Incluir revisión del README histórico si se autoriza dentro del alcance documental.

### Etapa 5 — Trazabilidad y revisión final de documentación

**Estado:** comprobaciones iniciales realizadas; revisión final pendiente.

1. Verificar consistencia entre índice y ADR individuales, arquitectura,
   requisitos, hitos, deuda, pruebas, investigación y AGENTS. Revisar enlaces,
   identificadores, fechas y la separación aceptado/propuesto/implementado.
2. Ejecutar `git diff --check` y `git diff --cached --check`, revisar diff y
   `git status --short --untracked-files=all`.
3. Incluir documentos sin seguimiento en la revisión: el diff normal no los
   cubre. Preferir `git diff --no-index --check` contra un archivo temporal vacío
   fuera del repositorio, sin tocar staging. No interpretar el código 1 de
   no-index por diferencias como error de whitespace; inspeccionar diagnósticos.
4. Conservar el intent-to-add existente; no ejecutar `git add -N` ni agregar
   contenido automáticamente para ampliar la revisión.
5. Revisar el contenido nuevo y comprobar que solo cambiaron archivos
   autorizados. No ejecutar build o llamadas pagadas por un cambio documental.
6. Actualizar este contexto con evidencia, límites y siguiente etapa; presentar
   el resultado y detenerse para revisión del usuario.

**Salida:** documentos coherentes, controles de formato y enlaces revisados,
riesgos pendientes explícitos y diff concreto para revisión.

### Etapa 6 — Commit y Pull Request

**Estado:** autorizado por excepción un primer commit local de los 23 documentos
antes de las investigaciones pendientes. La publicación por push/PR y futuros
commits siguen pendientes de autorización explícita. Esta excepción no da por
consolidadas las etapas 2–5 ni por completada toda la etapa 6.

Tras consolidación y revisión, presentar archivos exactos, propuesta de mensaje
de commit y descripción de PR con validaciones y límites. Solo con autorización
realizar las operaciones de Git o publicación que el usuario haya permitido;
autorizar un commit no autoriza por sí solo push, PR ni merge. Conservar etiqueta
y línea base v0.1.0; excluir secretos y artefactos locales.

### Etapa 7 — Codex Cloud y backlog de GitHub Issues

**Estado:** pendiente de consolidación documental y autorización explícita.

Primero preparar configuración revisable: repositorio/rama, setup reproducible
según scripts existentes, mocks sin claves, red y variables por nombre, límites
de secretos y validaciones. No configurar el entorno remoto todavía.

Preparar después un backlog trazable a RF/RNF, ADR, TD y M1–M6: alcance mínimo,
dependencias, criterios de aceptación y evidencia esperada. Separar investigación,
decisión e implementación; evitar convertir propuestas en tareas de stack ya
elegido. Presentar borradores antes de crear Issues; no publicar automáticamente.

## 5. Registro de validación

### 5.1 Diagnóstico inicial y creación del plan (etapa 0)

- `git diff --check`: código 0; sin errores de whitespace en los cinco archivos
  con intent-to-add. El espacio sobrante histórico del roadmap no reaparece.
- `git diff --cached --check`: código 0; sin contenido staged que revisar.
- Los 17 documentos inicialmente sin seguimiento se comprobaron contra un
  archivo temporal vacío con `git diff --no-index --check`: sin diagnósticos
  de whitespace; código 1 por diferencias de archivos nuevos.
- Git advierte conversión LF → CRLF en documentos existentes; no se cambió su
  configuración ni se normalizaron archivos del usuario.
- Este nuevo plan se revisó contra el alcance solicitado y se comprobó con
  `git diff --no-index --check`: sin errores de whitespace. Sus ocho enlaces
  Markdown locales resuelven a archivos existentes.
- Se compararon hashes SHA-256 de los 22 documentos previos: ninguno cambió.
  El único archivo nuevo de esta sesión es `docs/working-context.md`, sin
  seguimiento. HEAD y el staging conservan el estado inicial.
- Los resultados funcionales históricos no se revalidaron.

### 5.2 Sincronización documental de ADR-011 (etapa 1)

- Archivos modificados: `AGENTS.md`, índice ADR, ADR-011, arquitectura,
  requisitos, roadmap, deuda técnica (solo TD-007), plan de pruebas y este
  contexto. Se revisaron las diferencias contra copias temporales del estado
  previo a la etapa, fuera del repositorio, para distinguir cambios del usuario.
- `git diff --check` y `git diff --cached --check`: código 0, sin errores de
  whitespace. Sin contenido nuevo en staging; se conserva el intent-to-add.
- Revisión adicional de los nueve documentos completos mediante
  `git diff --no-index --check` contra un archivo vacío temporal: sin errores
  de whitespace. El código 1 indica diferencias frente al archivo vacío.
  Para esa comprobación se desactivaron advertencias safecrlf solo en el
  comando; no se cambió configuración persistente ni se normalizaron archivos.
- Enlaces Markdown locales verificados; ninguna referencia TST indefinida ni
  identificadores TST duplicados en los documentos revisados.
- El plan contiene ahora 23 casos: los ocho nuevos, TST-016 a TST-023, están
  PENDING. Los estados de los 15 casos históricos no cambiaron. TST-022 es el
  control documental independiente de derechos, todavía sin revisión ejecutada.
- RF-009 continúa Propuesto; ADR-011 conserva su aceptación limitada al
  alcance y estrategia; TD-007 sigue abierto. Los demás ADR no se modificaron.
- Se compararon hashes de los 23 documentos previos a esta etapa: solo cambian
  los nueve autorizados; los otros 14 conservan su contenido. No se añaden
  archivos al repositorio durante esta etapa. HEAD y etiqueta siguen en
  `ea09e8c`; el árbol continúa con documentación local pendiente de commit.
- No se ejecutaron pruebas funcionales, build, inferencias o validación del
  catálogo: el cambio es documental y no acredita implementación, calidad,
  verificación nueva de la fuente SEP ni derechos de reutilización.
- Sin staging adicional, commit, push, PR, cambios funcionales, investigación
  de colas, configuración cloud ni creación de Issues.

## 6. Instrucción para retomar desde VS Code

> Lee AGENTS.md y docs/working-context.md. Comprueba rama, HEAD, etiqueta y
> estado de Git antes de editar. Conserva todos los cambios locales. La etapa
> 0 creó este plan y la etapa 1, autorizada por el usuario, completó la
> sincronización documental de ADR-011. Las validaciones del catálogo y los
> derechos siguen pendientes. La siguiente actividad propuesta es etapa 2:
> comparar colas/workers y revisar ADR-008, únicamente cuando el usuario la
> autorice. Se autorizó por excepción un primer commit local de los 23 documentos;
> push y PR siguen sin autorización. Las etapas 2–7 no se dan por realizadas
> por ese commit. Al terminar cada etapa,
> presenta diff y validaciones y detente en su punto de autorización.
> No implementes funcionalidades, cambies stack ni leas secretos. No hagas
> staging, commit, push, PR, configuración cloud o creación de Issues sin la
> autorización específica correspondiente.
