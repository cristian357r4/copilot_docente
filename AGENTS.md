# AGENTS.md --- Copilot Docente

Este archivo define las instrucciones de trabajo para agentes de código
que operen en este repositorio. Aplica a todo el proyecto, salvo que
exista un `AGENTS.md` más específico en un subdirectorio.

## 1. Contexto del proyecto

Copilot Docente es una aplicación web orientada a ayudar a docentes a
preparar planeaciones didácticas. La generación de IA debe producir un
borrador que el docente pueda revisar, corregir y adaptar. El docente
conserva el criterio y la decisión profesional.

La línea base funcional documentada es `v0.1.0`. El repositorio utiliza
Next.js y actualmente contiene, entre otros, estos puntos de entrada
observados:

-   `pages/api/generate.js`: endpoint de generación.
-   `pages/generar-planeacion.js`: interfaz de generación de
    planeaciones.

No presupongas versiones, scripts, dependencias, rutas adicionales ni
capacidades que no hayas verificado en el repositorio actual.

## 2. Antes de modificar código

1.  Inspecciona `git status --short` y conserva los cambios existentes
    del usuario.
2.  Lee `README.md`, `package.json` y los documentos relevantes de
    `docs/` antes de proponer cambios que afecten arquitectura,
    dependencias o comportamiento.
3.  Revisa los ADR relacionados con el cambio. Respeta su estado: una
    decisión `Proposed` no es una decisión aprobada.
4.  Inspecciona la implementación y las pruebas existentes antes de
    asumir cómo funciona una característica.
5.  Identifica el alcance mínimo necesario y los archivos que planeas
    modificar.
6.  Si falta información que pueda cambiar sustancialmente la solución,
    pregunta antes de tomar una decisión irreversible.

No sobrescribas cambios locales ni asumas que el código del repositorio
original/upstream es más correcto que el estado actual del fork.

## 3. Restricciones de producto y arquitectura

### 3.1 Inferencia de modelos

-   La inferencia del producto en producción debe realizarse mediante
    servicios cloud.
-   Los modelos locales pueden utilizarse para desarrollo,
    experimentación y evaluación de costos; no diseñes el producto para
    exigir que los docentes instalen o ejecuten modelos en sus equipos.
-   No agregues dependencias de instalación, hardware especializado o
    ejecución local de modelos para el usuario docente.
-   Mantén la lógica de negocio independiente del proveedor de IA.
-   NVIDIA NIM y OpenAI son proveedores contemplados por el proyecto. No
    afirmes que ambos están validados de extremo a extremo sin consultar
    `docs/testing/test-plan.md` y ejecutar las pruebas correspondientes.
-   Lee la configuración desde variables de entorno. Nunca incrustes API
    keys, tokens u otras credenciales en código, documentación, fixtures
    o ejemplos.
-   No cambies proveedor, modelo, endpoint, parámetros de inferencia o
    comportamiento de selección sin que el alcance de la tarea lo
    autorice.
-   No añadas un nuevo proveedor sin revisar el contrato existente, los
    ADR y las pruebas pertinentes.

### 3.2 Resultados de generación y calidad

-   Trata la respuesta del modelo como contenido no confiable hasta
    validar su estructura y los datos que consume la interfaz.
-   No presentes una respuesta de fallback, plantilla o demostración
    como si fuera una inferencia real.
-   Si existe fallback, debe quedar identificado de forma inequívoca
    tanto para el usuario como para la instrumentación.
-   No conviertas un error de proveedor, parsing o validación en una
    respuesta aparentemente exitosa sin una decisión explícita y
    pruebas.
-   No afirmes que una planeación está alineada con un programa
    curricular oficial si no existe una fuente identificada, vigente y
    verificable que respalde esa afirmación.
-   Conserva la posibilidad de revisión y edición por parte del docente.
-   No optimices únicamente por longitud o apariencia del texto:
    considera coherencia entre objetivos, actividades, secuencia,
    evaluación, grado y duración.

### 3.3 Arquitectura y alcance

-   Evita introducir microservicios, colas, agentes autónomos,
    multiagentes, RAG, bases de datos o infraestructura distribuida por
    anticipado.
-   Antes de implementar persistencia durable, idempotencia,
    recuperación de trabajos, facturación o BYOK, revisa los requisitos
    y ADR correspondientes.
-   Distingue las decisiones aprobadas de las alternativas en
    investigación.
-   Si el cambio requiere una decisión arquitectónica nueva, documenta
    las opciones y consecuencias y propone un ADR. No marques
    unilateralmente una propuesta como `Accepted`.
-   Prefiere cambios pequeños y compatibles con la arquitectura
    existente.

### 3.4 Alcance curricular del MVP (ADR-011)

-   Respeta `docs/adr/0011-curriculum-knowledge.md`: exclusivamente
    quinto de primaria en México. El alcance aceptado no acredita que
    el catálogo esté implementado o validado; RF-009 sigue Propuesto.
-   La estrategia aceptada es un catálogo estructurado y versionado,
    con filtros y relaciones explícitas entre grado, campo formativo,
    contenido y PDA. El almacenamiento físico permanece pendiente.
-   Cubre Lenguajes; Saberes y Pensamiento Científico; Ética, Naturaleza
    y Sociedades; y De lo Humano y lo Comunitario. No mezcles contenidos
    o PDA exclusivos de sexto con los de quinto.
-   Conserva fuente oficial, edición, página y versión del catálogo.
    Distingue referencias oficiales de sugerencias generadas; no
    inventes relaciones o secuencias oficiales ni presentes la
    planeación como sustituto del Programa Analítico de la escuela.
-   RAG, embeddings y bases vectoriales están fuera del MVP. Solo
    reconsiderarlos con evidencia de necesidad y una nueva decisión.
-   Los derechos de reutilización siguen pendientes. No supongas
    autorización comercial por disponibilidad pública de las fuentes;
    resolver la revisión y, cuando corresponda, la autorización antes
    de distribuir contenido protegido en un producto comercial.
-   Consulta TST-016 a TST-023 en `docs/testing/test-plan.md` antes de
    afirmar cobertura curricular, alineación verificada o validación
    docente. Mantén su estado pendiente hasta contar con evidencia.

## 4. Protección de código y cambios del usuario

-   No restaures código eliminado por el usuario desde upstream, commits
    anteriores, ramas u otras versiones sin autorización explícita.
-   En particular, antes de modificar `pages/generar-planeacion.js`,
    compara el estado actual con el historial solo si es necesario para
    entender el cambio; no reincorpores automáticamente fragmentos
    eliminados.
-   No hagas refactors amplios cuando la solicitud sea corregir un
    defecto localizado.
-   No elimines funcionalidades, campos, validaciones, comentarios o
    documentación por considerarlos innecesarios sin justificarlo.
-   No cambies contratos de API ni formatos de respuesta sin revisar sus
    consumidores y pruebas.
-   No actualices versiones mayores de dependencias ni reemplaces
    bibliotecas sin explicar la necesidad, compatibilidad e impacto.
-   Si modificas `package.json`, revisa y actualiza `package-lock.json`
    de forma coherente. No regeneres el lockfile sin motivo.

## 5. Seguridad y secretos

-   `.env.local` contiene configuración local y no debe versionarse.
-   Mantén los archivos reales de secretos fuera de Git. Los ejemplos
    versionados deben contener únicamente nombres de variables y valores
    ficticios o placeholders.
-   No muestres valores de claves, tokens, cookies, encabezados
    `Authorization` ni secretos en terminales, logs, reportes, capturas
    o respuestas.
-   No copies secretos a issues, Pull Requests, documentación ni
    prompts.
-   No cambies ni revoques credenciales por iniciativa propia. Si
    detectas una posible exposición, informa el riesgo y recomienda la
    rotación al responsable.
-   Minimiza los datos personales enviados a los proveedores y evita
    incluir datos identificables de estudiantes en ejemplos o pruebas.
-   Sanitiza logs y evidencias antes de compartirlos.

## 6. Manejo de errores, solicitudes y costos

-   Distingue errores de validación, autenticación, cuota, proveedor,
    DNS, conexión, timeout, parsing y validación de respuesta.
-   No añadas reintentos automáticos a operaciones facturables sin
    analizar el riesgo de duplicar inferencias.
-   No supongas que cerrar o recargar el navegador cancela o conserva
    una generación. Verifica el comportamiento real.
-   No afirmes que existen persistencia, recuperación o idempotencia
    hasta que el código y las pruebas lo demuestren.
-   Cuando el proveedor reporte uso de tokens, conserva los metadatos
    necesarios sin registrar secretos ni contenido sensible.
-   No inventes precios ni costos si no hay una tarifa y fecha de
    referencia.

## 7. Pruebas y validación

Antes de ejecutar comandos, inspecciona `package.json` y utiliza los
scripts que realmente existan. No inventes comandos de pruebas.

Según el alcance del cambio:

1.  Ejecuta las pruebas específicas existentes.
2.  Añade o actualiza pruebas para el comportamiento modificado cuando
    sea viable.
3.  Ejecuta `npm run build` cuando el cambio pueda afectar la
    compilación o las rutas de la aplicación.
4.  Si una prueba no puede ejecutarse, explica por qué; no la marques
    como aprobada.
5.  Revisa el diff y `git status` al terminar.

Mantén actualizado `docs/testing/test-plan.md` cuando se ejecute una
prueba relevante. Registra proveedor, modelo, commit, configuración,
resultado y evidencia sanitizada cuando corresponda.

No marques una prueba como `PASS` solo porque una API respondió HTTP
200. Separa conectividad, integración, renderizado y calidad pedagógica.

## 8. Documentación y trazabilidad

Consulta y conserva la separación de responsabilidades:

-   `docs/architecture.md`: contexto y arquitectura.
-   `docs/requirements.md`: requisitos.
-   `docs/roadmap.md`: hitos y dependencias.
-   `docs/technical-debt.md`: hallazgos, riesgos y deuda técnica.
-   `docs/adr/`: decisiones y alternativas arquitectónicas.
-   `docs/testing/`: plan, casos y resultados de pruebas.
-   `docs/research/`: investigación, entrevistas y comparativas.

Cuando un cambio altere requisitos, arquitectura, comportamiento o
estado de una deuda técnica, actualiza el documento correspondiente. No
dupliques información innecesariamente ni cambies el estado de un
hallazgo sin evidencia.

## 9. Reglas de Git

-   Trabaja en la rama actual, salvo instrucción explícita para
    cambiarla.
-   No cambies de rama para ocultar o evitar cambios existentes.
-   No ejecutes `git reset --hard`, `git clean -fd`, rebase destructivo,
    `push --force` ni comandos equivalentes sin autorización explícita.
-   No descartes cambios locales del usuario.
-   No hagas `git add .` por defecto. Selecciona los archivos
    correspondientes a la tarea y revisa el staging.
-   No crees commits, tags, ramas remotas, Pull Requests ni merges sin
    que el usuario lo solicite.
-   Nunca incluyas `.env.local`, credenciales, archivos temporales o
    artefactos de build en un commit.
-   Antes de finalizar, informa la rama, los archivos modificados y el
    estado del árbol de trabajo.

## 10. Forma de trabajo y respuesta

Para cada tarea:

1.  Resume brevemente qué entendiste y cuál es el alcance.
2.  Inspecciona el código y los documentos relevantes.
3.  Propón un plan corto cuando el cambio afecte varios archivos o
    comportamiento.
4.  Implementa el cambio mínimo necesario.
5.  Ejecuta las validaciones pertinentes.
6.  Revisa el diff para detectar cambios accidentales.
7.  Informa:
    -   qué cambió;
    -   qué archivos se modificaron;
    -   qué pruebas se ejecutaron y sus resultados;
    -   qué no se pudo validar;
    -   riesgos o decisiones que requieren aprobación.

No afirmes que una tarea está terminada si no se ejecutaron las
validaciones relevantes. No ocultes fallos ni completes vacíos de
información con suposiciones.

## 11. Precedencia

Si las instrucciones de una tarea contradicen este archivo, detente y
explica el conflicto antes de realizar cambios de alto impacto. Las
instrucciones explícitas del usuario pueden autorizar una excepción
concreta, pero no deben interpretarse como autorización para acciones
destructivas o exposición de secretos que no se hayan solicitado.

Los requisitos y ADR vigentes son la referencia para decisiones del
producto. Este archivo establece cómo trabajar en el repositorio; no
reemplaza ni aprueba decisiones arquitectónicas.
