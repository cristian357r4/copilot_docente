# Deuda técnica --- Copilot Docente

> **Estado:** inventario inicial; requiere revisión contra el código
> actual.\
> **Fecha de registro:** 26 de noviembre de 2026\
> **Línea base:** `v0.1.0`\
> **Regla:** este documento distingue hallazgos observados, riesgos
> inferidos y aspectos aún no auditados. No implica autorización para
> resolverlos todos en un mismo cambio.

## 1. Cómo interpretar este inventario

-   **Confirmado:** se observó directamente durante las pruebas o en la
    salida de comandos compartida.
-   **Por verificar:** existe una señal o riesgo, pero hace falta
    inspeccionar el repositorio o ejecutar una prueba específica.
-   **No demostrado:** no se ha presentado evidencia de que la capacidad
    esté implementada y funcionando. No equivale a afirmar que no exista
    código.

Prioridades:

-   **P0 --- Seguridad/bloqueante:** resolver o mitigar antes de exponer
    el sistema a usuarios externos.
-   **P1 --- Alta:** resolver antes de un piloto con docentes o antes de
    depender de la capacidad.
-   **P2 --- Media:** resolver durante la consolidación del MVP.
-   **P3 --- Baja:** mejora que puede esperar mientras no afecte el
    flujo principal.

## 2. Inventario

### TD-001 --- Rotación y auditoría de credenciales NVIDIA

-   **Prioridad:** P0
-   **Estado:** Por verificar; acción preventiva necesaria.
-   **Evidencia:** el proyecto compartido durante la conversación
    incluía una API key. Se revisaron archivos de documentación y
    `.env.local` aparece ignorado por Git
    (`git check-ignore .env.local`). También se hizo una búsqueda de
    patrones en `HEAD`, pero esa búsqueda no demuestra que todos los
    secretos estén ausentes del historial completo.
-   **Riesgo:** una clave que haya sido compartida o comprometida puede
    permitir consumo no autorizado y generar costos.
-   **Acciones:**
    1.  Revocar o rotar la clave NVIDIA que se incluyó en el archivo
        compartido, si sigue activa.
    2.  Confirmar que `.env.local` y cualquier archivo de secretos estén
        ignorados y no estén versionados.
    3.  Revisar el árbol de trabajo, archivos no rastreados, ramas y el
        historial Git completo en busca de secretos reales.
    4.  No copiar claves, tokens ni valores secretos a issues, logs,
        documentación o prompts de agentes.
    5.  Si se confirma un secreto real en el historial remoto, definir
        un procedimiento de limpieza del historial y coordinación antes
        de reescribirlo.
-   **Criterio de cierre:** clave expuesta revocada; archivos locales
    ignorados; escaneo de secretos documentado; ningún secreto activo
    confirmado en el repositorio o historial.

### TD-002 --- Error intermitente `ENOTFOUND` al llamar a NVIDIA

-   **Prioridad:** P1
-   **Estado:** Intermitencia observada; causa raíz no identificada.
-   **Evidencia:** una petición desde la aplicación terminó con
    `ENOTFOUND`. Posteriormente `Resolve-DnsName`, `dns.lookup` de
    Node.js y una petición Axios a `/models` funcionaron. La inferencia
    directa y la generación desde la aplicación también funcionaron
    después.
-   **Riesgo:** fallos intermitentes de generación y diagnóstico
    incorrecto si se atribuye todo a DNS.
-   **Acciones:**
    1.  Capturar timestamp, hostname, código de error, fase de la
        petición y proveedor, sin registrar credenciales.
    2.  Repetir pruebas desde el proceso Next.js y desde un proceso
        Node.js independiente bajo condiciones comparables.
    3.  Inspeccionar configuración de proxy, DNS, IPv4/IPv6, resolución
        y agente HTTP cuando vuelva a ocurrir.
    4.  Distinguir `ENOTFOUND`, `ECONNRESET`, timeout, TLS y errores
        HTTP.
    5.  Definir reintentos limitados solo después de determinar qué
        errores son transitorios y cómo evitar duplicar inferencias
        facturables.
-   **Criterio de cierre:** causa raíz identificada o mitigación
    reproducible, con pruebas y logs sanitizados.

### TD-003 --- Integración OpenAI no validada de extremo a extremo

-   **Prioridad:** P1
-   **Estado:** Pendiente.
-   **Evidencia:** el formulario permite seleccionar OpenAI y NVIDIA
    NIM; durante esta sesión se validó NVIDIA, pero no se mostró una
    generación completa usando OpenAI.
-   **Riesgo:** un proveedor visible en la interfaz puede fallar por
    configuración, inicialización, formato de solicitud o parsing.
-   **Acciones:** verificar variables de entorno, inicialización del
    cliente, modelo configurado, petición, errores, respuesta
    estructurada y renderizado desde la aplicación.
-   **Criterio de cierre:** caso de prueba reproducible de OpenAI que
    complete el flujo y documente consumo y errores.

### TD-004 --- Fallback puede ocultar errores de inferencia

-   **Prioridad:** P1
-   **Estado:** Hallazgo observado en una salida de código; verificar el
    estado actual.
-   **Evidencia:** durante la inspección de `pages/api/generate.js` se
    observó `getFallbackResponse(materia, grado)` y llamadas al fallback
    ante proveedor no disponible/configuración inválida y ante error al
    parsear la respuesta. En el fragmento observado, una de esas rutas
    respondía con HTTP 200.
-   **Riesgo:** la interfaz puede presentar una planeación predefinida
    como si fuera una generación real. Esto contamina las pruebas de
    calidad, dificulta medir disponibilidad y puede inducir a error al
    docente.
-   **Acciones:**
    1.  Inspeccionar el archivo actual y confirmar todas las rutas que
        llaman al fallback.
    2.  Definir si el fallback es una demo explícita, una plantilla de
        emergencia o si debe retirarse.
    3.  Nunca etiquetar una respuesta predefinida como resultado de
        inferencia real.
    4.  Usar estados y códigos de error coherentes; registrar
        `provider`, `model`, `fallbackUsed` y `requestId` sin datos
        sensibles.
    5.  Añadir pruebas para proveedor inválido, falta de credenciales,
        error HTTP y JSON inválido.
-   **Criterio de cierre:** el usuario y las métricas distinguen
    inequívocamente una inferencia real de una plantilla/fallback; las
    pruebas verifican cada ruta.

### TD-005 --- Validación estructural insuficiente de la respuesta del modelo

-   **Prioridad:** P1
-   **Estado:** Por verificar/ampliar.
-   **Evidencia:** se observó uso de `JSON.parse` para interpretar la
    respuesta. No se mostró una validación posterior mediante esquema.
-   **Riesgo:** JSON sintácticamente válido puede omitir secciones, usar
    tipos incorrectos o incumplir el contrato esperado por la interfaz.
-   **Acciones:** definir un contrato versionado para la planeación;
    validar con JSON Schema o una biblioteca equivalente; tratar fallos
    de validación como errores explícitos; crear fixtures de respuestas
    válidas e inválidas.
-   **Criterio de cierre:** toda respuesta se valida antes de
    renderizarse y los errores de esquema tienen pruebas.

### TD-006 --- Calidad pedagógica sin rúbrica ni conjunto de evaluación

-   **Prioridad:** P1
-   **Estado:** Necesidad confirmada; evaluación sistemática pendiente.
-   **Evidencia:** la primera planeación generada fue estructuralmente
    útil, pero el problema inicial utilizó `3/4` y `1/4` y después
    indicó que la dificultad estaba en denominadores distintos. La
    entrada del caso trataba suma y resta de fracciones con diferentes
    denominadores.
-   **Riesgo:** texto convincente con inconsistencias matemáticas o
    pedagógicas.
-   **Acciones:** definir una rúbrica con docentes; crear casos
    representativos por nivel, grado y asignatura; evaluar coherencia,
    exactitud disciplinar, secuencia, evaluación y viabilidad; conservar
    resultados de cada modelo y versión de prompt.
-   **Criterio de cierre:** rúbrica revisada por docentes, conjunto de
    evaluación reproducible y resultados comparables entre modelos.

### TD-007 --- Alineación curricular y fuentes oficiales no verificadas

-   **Prioridad:** P1
-   **Estado:** Alcance y estrategia aceptados en ADR-011; construcción,
    verificación curricular, revisión docente y derechos pendientes.
-   **Evidencia:** la planeación contiene un campo formativo, pero no se
    presentó evidencia de recuperación o validación contra documentos
    curriculares oficiales. ADR-011 fija el catálogo estructurado y
    versionado de quinto de primaria en México; esa decisión documental
    no acredita que el catálogo esté implementado o validado.
-   **Riesgo:** atribuir alineación con SEP u otro marco curricular sin
    comprobar versión, vigencia y correspondencia; mezclar PDA de sexto
    o asumir derechos de reutilización comercial todavía no resueltos.
-   **Acciones:** verificar la fuente SEP y su edición registradas en
    ADR-011; construir y validar el catálogo de los cuatro campos de
    quinto con relaciones explícitas contenido/PDA, excluyendo los
    exclusivos de sexto. Conservar fuente, edición, página y versión
    del catálogo. Distinguir referencias oficiales de sugerencias
    generadas y revisarlas con docentes de quinto. Resolver la revisión
    de derechos y, cuando corresponda, la autorización antes de distribuir
    contenido protegido en un producto comercial. No reabrir catálogo
    frente a RAG: RAG, embeddings y bases vectoriales quedan fuera del MVP;
    reconsiderarlos solo con evidencia de necesidad y una nueva decisión.
-   **Criterio de cierre:** evidencia de TST-016 a TST-023 en el
    [plan de pruebas](testing/test-plan.md), vinculada a RF-009/RNF-010
    y ADR-011: cobertura, separación de grados, relaciones, referencias,
    revisión docente y condiciones de reutilización documentadas. Sigue
    abierto; la sincronización documental no equivale a su cierre.

### TD-008 --- Persistencia y recuperación de generaciones no demostradas

-   **Prioridad:** P1
-   **Estado:** No demostrado en la línea base.
-   **Evidencia:** se discutió la necesidad de recuperar una generación
    tras cerrar o recargar la página, pero no se presentó una prueba de
    persistencia de trabajos ni de reanudación.
-   **Riesgo:** pérdida de resultados, peticiones repetidas y cobros
    adicionales cuando el usuario pierde conexión.
-   **Acciones:** definir ciclo de vida y estados de un trabajo;
    determinar qué se persiste y durante cuánto tiempo; evaluar
    recuperación tras recarga, cierre de pestaña, reinicio del servidor
    y timeout del proveedor.
-   **Criterio de cierre:** pruebas que demuestren el comportamiento
    esperado ante desconexión y reapertura.

### TD-009 --- Idempotencia y prevención de inferencias duplicadas no demostradas

-   **Prioridad:** P1
-   **Estado:** Propuesta arquitectónica; implementación no demostrada.
-   **Evidencia:** se discutieron claves de idempotencia y
    deduplicación, pero no se presentó una prueba de solicitudes
    concurrentes o reintentos.
-   **Riesgo:** doble generación y doble costo por reintentos del
    navegador o del usuario.
-   **Acciones:** definir el alcance de la clave de idempotencia, su
    almacenamiento, expiración, asociación con usuario y hash de
    entrada; especificar qué ocurre con solicitudes idénticas y con
    solicitudes que difieren.
-   **Criterio de cierre:** pruebas concurrentes y de reintento
    demuestran que una misma operación lógica no produce trabajos
    duplicados.

### TD-010 --- Contrato y abstracción de proveedores por auditar

-   **Prioridad:** P2
-   **Estado:** Parcialmente implementado; alcance por confirmar.
-   **Evidencia:** la interfaz permite elegir OpenAI o NVIDIA NIM y la
    aplicación utiliza configuración NVIDIA por entorno. No se ha
    documentado una prueba de contrato común que cubra ambos
    proveedores.
-   **Riesgo:** condicionales y formatos específicos de cada proveedor
    pueden propagarse por la lógica de negocio.
-   **Acciones:** inspeccionar la implementación; definir interfaz común
    para generación, errores, uso y capacidades; separar identificador
    de proveedor, modelo y configuración; probar cada adaptador.
-   **Criterio de cierre:** la lógica de planeación no depende de
    detalles particulares del SDK y los adaptadores cumplen el mismo
    contrato.

### TD-011 --- Métricas y atribución de costos no integradas en el producto

-   **Prioridad:** P2
-   **Estado:** Datos disponibles en una prueba aislada; instrumentación
    de producto no demostrada.
-   **Evidencia:** la inferencia directa devolvió tokens de entrada,
    salida y total. No se mostró persistencia ni agregación de esas
    métricas por solicitud o usuario.
-   **Riesgo:** desconocer el costo por planeación útil, detectar tarde
    un consumo anómalo o comparar modelos con métricas incompletas.
-   **Acciones:** registrar `requestId`, proveedor, modelo, tokens
    reportados, latencia, resultado y estado; mantener tarifas
    versionadas por proveedor/modelo; no inferir costo si faltan datos
    de precio.
-   **Criterio de cierre:** reporte reproducible de consumo y costo
    estimado por solicitud, con supuestos explícitos.

### TD-012 --- Autenticación, autorización, privacidad y retención no auditadas

-   **Prioridad:** P0 antes de piloto con datos reales
-   **Estado:** No auditado.
-   **Evidencia:** no se presentó una revisión de autenticación,
    separación de datos, autorización, retención o eliminación.
-   **Riesgo:** acceso indebido o tratamiento inadecuado de información
    de docentes, estudiantes o instituciones.
-   **Acciones:** definir datos personales que se aceptan; minimizar
    datos de estudiantes; revisar autenticación y autorización; definir
    aislamiento por usuario/organización, retención, eliminación,
    exportación y logs; realizar revisión de seguridad antes de piloto.
-   **Criterio de cierre:** controles y políticas documentados y
    probados para el alcance del piloto.

### TD-013 --- Dashboard y funcionalidades anunciadas sin comportamiento validado

-   **Prioridad:** P2
-   **Estado:** Por verificar.
-   **Evidencia:** la pantalla inicial muestra tarjetas de planeaciones
    recientes, dosificaciones y calendario; no se presentó evidencia de
    que esas tarjetas estén conectadas a persistencia o flujos
    funcionales.
-   **Riesgo:** la interfaz puede crear expectativas de capacidades que
    todavía son solo estructura visual.
-   **Acciones:** inventariar rutas y controles; etiquetar lo que es
    placeholder; decidir con docentes qué capacidades pertenecen al MVP;
    ocultar o deshabilitar elementos sin función.
-   **Criterio de cierre:** cada elemento visible tiene comportamiento
    definido o está claramente identificado como no disponible.

### TD-014 --- Advertencia deprecada de Node.js (`util._extend`)

-   **Prioridad:** P3
-   **Estado:** Confirmado durante `npm start`; origen no identificado.
-   **Evidencia:** Node mostró `DEP0060` indicando que `util._extend`
    está deprecado y recomendó `Object.assign()`.
-   **Riesgo:** deuda en código propio o dependencia transitiva; no se
    ha demostrado impacto funcional.
-   **Acciones:** ejecutar con `node --trace-deprecation`, identificar
    el paquete que genera la advertencia y actualizarlo de forma
    controlada. No modificar dependencias sin verificar compatibilidad.
-   **Criterio de cierre:** origen identificado y advertencia resuelta o
    justificada/documentada.

### TD-015 --- Cobertura automatizada no evidenciada

-   **Prioridad:** P2
-   **Estado:** Por verificar.
-   **Evidencia:** se compartió una compilación exitosa y pruebas
    manuales de integración; no se mostraron resultados de suites
    unitarias, de integración o E2E para este cambio.
-   **Riesgo:** regresiones en validación, parsing, manejo de errores y
    selección de proveedores.
-   **Acciones:** inspeccionar scripts y suites existentes antes de
    añadir herramientas; definir pruebas unitarias para normalización y
    validación, integración para adaptadores y E2E para el flujo
    principal.
-   **Criterio de cierre:** pruebas automatizadas relevantes ejecutables
    localmente y en CI, con resultados documentados.

## 3. Orden sugerido de atención

  -----------------------------------------------------------------------
  Orden                   Elementos               Motivo
  ----------------------- ----------------------- -----------------------
  Antes de compartir el   TD-001, TD-012          Credenciales y datos
  repositorio o piloto                            personales

  Antes de ampliar        TD-004, TD-005, TD-006, Evitar resultados
  pruebas de IA           TD-007                  engañosos o incorrectos

  Antes de un piloto      TD-002, TD-003, TD-008, Fiabilidad y control de
  operativo               TD-009                  costos

  Durante consolidación   TD-010, TD-011, TD-013, Mantenibilidad y
  del MVP                 TD-015                  experiencia

  Mejora de mantenimiento TD-014                  Advertencia deprecada
  -----------------------------------------------------------------------

El orden no autoriza a mezclar todas las correcciones en un solo PR.
Cada cambio debe tener alcance, pruebas y revisión propios.

## 4. Relación con otros documentos

-   `docs/architecture.md`: contexto y arquitectura.
-   `docs/requirements.md`: requisitos y criterios de aceptación.
-   `docs/roadmap.md`: hitos, dependencias y criterios de salida.
-   `docs/adr/`: decisiones aprobadas y propuestas.
-   `docs/research/`: investigación, entrevistas y comparativas.
-   `docs/testing/`: casos de prueba, rúbricas y resultados.

## 5. Regla de mantenimiento

Al resolver un elemento:

1.  Añadir evidencia del cambio (PR, commit, prueba o decisión).
2.  Actualizar su estado a `Resolved`, `Accepted risk` o `Superseded`.
3.  No borrar el historial del hallazgo.
4.  Si la solución requiere una decisión arquitectónica, actualizar o
    crear el ADR correspondiente.
5.  Si se divide en trabajo, crear GitHub Issues enlazadas al
    identificador `TD-xxx`.

Este inventario debe revisarse contra el código real antes de tratarlo
como auditoría exhaustiva.
