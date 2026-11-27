# Plan de pruebas --- Copilot Docente

> **Línea base:** `v0.1.0`\
> **Fecha de referencia:** 26 de noviembre de 2026\
> **Entorno observado:** ejecución local en Windows mediante PowerShell,
> aplicación Next.js en `http://localhost:3000`.\
> **Estado del documento:** inventario inicial; debe actualizarse al
> repetir pruebas y al incorporar automatización.

## 1. Propósito

Establecer un registro reproducible para validar el flujo de Copilot
Docente y comparar proveedores/modelos sin depender de impresiones
subjetivas.

Este documento separa:

-   **Evidencia ejecutada:** pruebas que se realizaron y cuyo resultado
    fue visible durante la sesión.
-   **Pendiente de ejecutar:** pruebas necesarias para las que todavía
    no existe evidencia.
-   **Observaciones de calidad:** aspectos del resultado que requieren
    evaluación, aunque la solicitud haya terminado correctamente.

Una respuesta HTTP exitosa o una compilación correcta no demuestran por
sí solas que la planeación sea pedagógicamente adecuada.

## 2. Alcance de la línea base

La línea base `v0.1.0` incluye la selección de proveedor en la interfaz,
la integración probada con NVIDIA NIM y la generación de una planeación
desde el formulario.

El formulario observado contiene:

-   Proveedor de IA: OpenAI o NVIDIA NIM.
-   Nivel educativo.
-   Grado.
-   Materia/asignatura.
-   Campo formativo.
-   Contenido/tema.
-   Objetivo de aprendizaje.
-   Duración en semanas.
-   Número de sesiones.

El resultado observado presenta secciones de Inicio, Desarrollo, Cierre
y Evaluación.

No se considera validado por esta línea base: persistencia de trabajos,
recuperación tras cerrar la página, idempotencia, autenticación,
operación multiusuario, integración completa de OpenAI ni alineación
curricular oficial.

## 3. Entorno y datos de prueba

### 3.1 Entorno observado

  -----------------------------------------------------------------------------
  Elemento                            Valor/evidencia
  ----------------------------------- -----------------------------------------
  Sistema                             Windows, terminal PowerShell

  Aplicación                          Next.js

  URL local                           `http://localhost:3000`

  Archivo de entorno                  `.env.local` cargado por Next.js

  Proveedor probado en la aplicación  NVIDIA NIM

  Modelo reportado por el catálogo    `nvidia/nemotron-3.5-lightning-30b-a3b`

  Build                               `npm run build` completado correctamente

  Advertencia observada               Node `DEP0060`: `util._extend` está
                                      deprecado
  -----------------------------------------------------------------------------

La versión exacta de Node.js, sistema operativo y dependencias debe
registrarse en una nueva ejecución. No se debe copiar el contenido de
`.env.local` al reporte.

### 3.2 Caso funcional de referencia

**Identificador del escenario: TC-AI-001.** Este es el caso citado en
arquitectura y RF-001; TST-005 registra su ejecución histórica con NVIDIA.
TC identifica el escenario y TST identifica la prueba registrada; no son dos
ejecuciones diferentes.

Se utilizó el siguiente escenario para una generación manual:

  -----------------------------------------------------------------------
  Campo                               Valor utilizado
  ----------------------------------- -----------------------------------
  Proveedor                           NVIDIA NIM

  Nivel educativo                     Primaria

  Grado                               5

  Materia                             Matemáticas

  Campo formativo                     Saberes y Pensamiento Científico

  Contenido/tema                      Representación, comparación, suma y
                                      resta de fracciones con diferentes
                                      denominadores mediante situaciones
                                      de la vida cotidiana

  Objetivo                            Que los estudiantes comprendan la
                                      equivalencia de fracciones,
                                      resuelvan operaciones de suma y
                                      resta y apliquen estos
                                      conocimientos para solucionar
                                      problemas cotidianos

  Duración                            1 semana

  Sesiones                            5
  -----------------------------------------------------------------------

Este escenario debe conservarse como caso de regresión, sin asumir que
una única ejecución representa la calidad general del modelo.

Su tema de fracciones tampoco se considera alineado con un PDA oficial por
el hecho de haberse generado para quinto: esa correspondencia requiere las
validaciones curriculares pendientes y es distinta de la revisión matemática
de TST-009. No se alteran los datos ni los resultados históricos del caso.

### 3.3 Alcance de la validación curricular pendiente

[ADR-011](../adr/0011-curriculum-knowledge.md) acepta un catálogo estructurado
y versionado exclusivamente de quinto de primaria en México, sin RAG,
embeddings ni base vectorial en el MVP; no fija su almacenamiento físico.
TST-016 a TST-023 son criterios propuestos para RF-009/RNF-010 y TD-007,
no evidencia de que exista el catálogo ni de una alineación ya comprobada.
La revisión de derechos de TST-022 es independiente de las pruebas técnicas.

Antes de ejecutarlos deberán existir una versión identificada del catálogo,
la fuente oficial y su edición verificadas y los artefactos necesarios para
cada caso. No se incorporan aquí textos oficiales ni datos de estudiantes.
En esta etapa documental no se ejecutó ninguno: todos quedan PENDING.

## 4. Estados de las pruebas

-   **PASS:** el resultado esperado se observó en la ejecución
    registrada.
-   **FAIL:** el resultado observado incumplió el criterio.
-   **INTERMITTENT:** el resultado varió entre intentos o depende de una
    condición todavía no aislada.
-   **PENDING:** todavía no se ejecutó o no hay evidencia suficiente.
-   **NEEDS REVIEW:** la operación terminó, pero el contenido requiere
    evaluación funcional o pedagógica.

Los estados describen la evidencia disponible para esta línea base; no
sustituyen una nueva ejecución después de cambios de código,
configuración o modelo.

## 5. Registro de pruebas y evidencia

### TST-001 --- Compilación de producción

-   **Categoría:** build
-   **Estado:** PASS
-   **Ejecución:** `npm run build`
-   **Resultado observado:** Next.js compiló correctamente, recopiló
    datos de páginas y generó las páginas estáticas.
-   **Evidencia:** salida de terminal compartida el 26-11-2026.
-   **Límite:** confirma que el build terminó; no valida el
    comportamiento de todas las rutas en ejecución.

### TST-002 --- Inicio del servidor local

-   **Categoría:** smoke test
-   **Estado:** PASS, con advertencia
-   **Ejecución:** `npm start`
-   **Resultado observado:** servidor iniciado en `0.0.0.0:3000`; la
    aplicación abrió localmente.
-   **Advertencia:** Node mostró `DEP0060`, indicando que `util._extend`
    está deprecado.
-   **Pendiente asociado:** identificar el origen de la advertencia; ver
    `TD-014` en `docs/technical-debt.md`.

### TST-003 --- Consulta del catálogo de NVIDIA NIM

-   **Categoría:** integración de proveedor
-   **Estado:** PASS
-   **Resultado observado:** petición al endpoint de modelos con
    respuesta HTTP 200; se reportaron 30 modelos disponibles y
    coincidencia con `nvidia/nemotron-3.5-lightning-30b-a3b`.
-   **Alcance:** prueba directa desde Node/Axios con la configuración
    local.
-   **Límite:** no demuestra por sí sola que todas las rutas de la
    aplicación estén configuradas correctamente.

### TST-004 --- Inferencia directa con NVIDIA NIM

-   **Categoría:** integración de proveedor
-   **Estado:** PASS
-   **Resultado observado:** una solicitud de prueba devolvió
    `Hola, ¿cómo estás?`.
-   **Uso reportado:** 29 tokens de entrada, 17 de salida y 46 en total.
-   **Alcance:** prueba directa del proveedor, no una medición de
    calidad pedagógica.

### TST-005 --- Generación desde el formulario con NVIDIA NIM

-   **Categoría:** smoke test de extremo a extremo
-   **Estado:** PASS como prueba funcional básica
-   **Escenario:** caso de referencia de quinto de primaria sobre
    operaciones con fracciones, TC-AI-001 (sección 3.2).
-   **Resultado observado:** la aplicación mostró una planeación con
    secciones Inicio, Desarrollo, Cierre y Evaluación.
-   **Límite:** una ejecución exitosa no demuestra consistencia entre
    múltiples ejecuciones ni calidad suficiente para uso docente.
-   **Revisión necesaria:** TST-009, coherencia matemática y pedagógica.

### TST-006 --- Resolución DNS y conectividad de NVIDIA

-   **Categoría:** conectividad
-   **Estado:** INTERMITTENT
-   **Resultado observado:** una generación desde la aplicación terminó
    con `ENOTFOUND`. Después, `Resolve-DnsName` y `dns.lookup`
    resolvieron `integrate.api.nvidia.com`; una petición Axios al
    catálogo respondió HTTP 200 y se logró ejecutar una inferencia.
-   **Conclusión permitida:** la conectividad funcionó en intentos
    posteriores.
-   **Conclusión no permitida:** no se ha identificado la causa raíz ni
    demostrado que el problema esté resuelto.
-   **Pendiente:** repetir y capturar diagnóstico cuando ocurra el
    fallo; relacionar con `TD-002`.

### TST-007 --- Generación completa con OpenAI

-   **Categoría:** integración de proveedor
-   **Estado:** PENDING
-   **Objetivo:** completar el mismo caso de referencia utilizando
    OpenAI desde la interfaz.
-   **Criterios:** configuración válida, solicitud aceptada, respuesta
    procesada, estructura válida, renderizado correcto y errores
    controlados.
-   **Nota:** la opción de OpenAI aparece en el formulario, pero no hay
    evidencia compartida de una generación completa con ese proveedor.

### TST-008 --- Respuesta inválida o malformada del modelo

-   **Categoría:** manejo de errores y contrato
-   **Estado:** PENDING
-   **Objetivo:** comprobar el comportamiento cuando el proveedor
    devuelve contenido vacío, JSON malformado o una estructura
    incompleta.
-   **Criterios:** no renderizar contenido inválido como resultado
    correcto; informar el fallo de forma comprensible; registrar
    diagnóstico sin secretos.
-   **Relación:** `TD-004` y `TD-005`.

### TST-009 --- Coherencia matemática y pedagógica de la planeación

-   **Categoría:** calidad del resultado
-   **Estado:** NEEDS REVIEW
-   **Observación concreta:** en la planeación mostrada se propuso como
    problema inicial sumar `3/4` y `1/4`, aunque el tema y el objetivo
    indicaban operaciones con fracciones de diferentes denominadores.
    Ese ejemplo tiene el mismo denominador en ambas fracciones.
-   **Interpretación:** es una señal específica que debe evaluarse; no
    basta para concluir que toda la planeación sea incorrecta.
-   **Criterios a evaluar:** correspondencia con el tema, exactitud
    matemática, progresión de actividades, adecuación al grado,
    alineación entre objetivos y evaluación, viabilidad en el tiempo.
-   **Pendiente:** construir una rúbrica revisada por docentes y un
    conjunto de casos. Relación: `TD-006` y `TD-007`.

### TST-010 --- Validación de campos del formulario

-   **Categoría:** interfaz y validación
-   **Estado:** PENDING
-   **Objetivo:** comprobar campos obligatorios, valores permitidos,
    límites numéricos, mensajes de validación y comportamiento ante
    entradas vacías o excesivas.
-   **Criterios:** no enviar solicitudes incompletas; mensajes claros;
    valores válidos enviados sin alteraciones inesperadas.
-   **Pendiente:** inspeccionar reglas reales del formulario y
    convertirlas en casos concretos.

### TST-011 --- Comportamiento del fallback

-   **Categoría:** manejo de errores
-   **Estado:** PENDING
-   **Objetivo:** comprobar si una respuesta de fallback se presenta de
    forma diferenciada de una inferencia real.
-   **Contexto:** durante una inspección anterior se observó una función
    `getFallbackResponse(materia, grado)` en `pages/api/generate.js` y
    rutas que la invocaban ante ciertos errores. Debe verificarse el
    código actual antes de afirmar que ese comportamiento sigue vigente.
-   **Criterios:** nunca presentar una plantilla o respuesta de
    demostración como si fuera una generación real; registrar claramente
    el motivo del fallback.
-   **Relación:** `TD-004`.

### TST-012 --- Recarga y cierre de página durante la generación

-   **Categoría:** resiliencia y experiencia
-   **Estado:** PENDING
-   **Objetivo:** comprobar qué ocurre si se recarga la página, se
    cierra la pestaña o se pierde la conexión mientras el proveedor
    procesa la solicitud.
-   **Criterios por definir:** estado visible al volver, recuperación
    del resultado, posibilidad de reintento seguro y ausencia de cobros
    duplicados evitables.
-   **Nota:** no se ha demostrado persistencia ni recuperación de
    trabajos en la línea base. Relación: `TD-008`.

### TST-013 --- Reintentos y solicitudes duplicadas

-   **Categoría:** idempotencia
-   **Estado:** PENDING
-   **Objetivo:** enviar solicitudes repetidas o concurrentes para
    comprobar si el sistema crea trabajos duplicados.
-   **Criterios por definir:** comportamiento de solicitudes idénticas,
    solicitudes distintas y reintentos tras timeout.
-   **Nota:** no se ha demostrado una implementación de idempotencia.
    Relación: `TD-009`.

### TST-014 --- Pruebas automatizadas y regresión

-   **Categoría:** automatización
-   **Estado:** PENDING
-   **Objetivo:** inspeccionar los scripts y pruebas existentes; después
    automatizar los casos críticos sin duplicar herramientas
    innecesariamente.
-   **Cobertura candidata:** validación de entrada, contrato de
    respuesta, parsing, adaptadores de proveedores y flujo principal.
-   **Criterio:** pruebas reproducibles localmente y, cuando
    corresponda, en CI; resultados registrados por commit.
-   **Relación:** `TD-015`.

### TST-015 --- Verificación de secretos en el repositorio

-   **Categoría:** seguridad
-   **Estado:** PARCIALMENTE VERIFICADO
-   **Resultado observado:** `git check-ignore .env.local` mostró que el
    archivo local está ignorado;
    `git ls-files -- .env.local components/nvidia.example.js` no mostró
    archivos rastreados en esa consulta. Se retiraron valores de
    credenciales de documentación y se hicieron búsquedas de patrones en
    el código y documentación.
-   **Límite:** una búsqueda por patrón no demuestra ausencia de
    secretos en todo el historial, archivos no rastreados, otras ramas o
    formatos.
-   **Pendiente:** escaneo integral de secretos y rotación de cualquier
    clave que haya sido expuesta. Relación: `TD-001`.

### TST-016 --- Integridad del catálogo de quinto y cuatro campos

-   **Categoría:** catálogo curricular; ADR-011, criterio 1.
-   **Estado:** PENDING
-   **Objetivo:** contrastar una versión identificada del catálogo con
    la fuente oficial para quinto de primaria en México.
-   **Procedimiento propuesto:** inventariar contenidos y PDA de quinto
    por campo y comparar registros con la fuente, conservando diferencias.
-   **Criterios:** cobertura sin omisiones o duplicados injustificados
    de Lenguajes; Saberes y Pensamiento Científico; Ética, Naturaleza y
    Sociedades; y De lo Humano y lo Comunitario. Un catálogo vacío o
    campos sin revisar no cumplen el criterio.
-   **Evidencia esperada:** inventario por campo, versión del catálogo,
    fuente/edición/páginas y reporte de conciliación de registros.

### TST-017 --- Exclusión de contenidos y PDA exclusivos de sexto

-   **Categoría:** separación de grados; ADR-011, criterio 2.
-   **Estado:** PENDING
-   **Objetivo:** evitar mezclar referencias de sexto con las de quinto.
-   **Procedimiento propuesto:** revisar las columnas o secciones de
    grado de la fuente y contrastarlas con los registros y filtros del
    catálogo; preparar casos negativos con referencias exclusivas de sexto.
-   **Criterios:** ningún contenido o PDA exclusivo de sexto se ofrece
    como propio de quinto. Para elementos compartidos debe existir
    evidencia explícita de su correspondencia con quinto.
-   **Evidencia esperada:** referencias de origen por grado y resultados
    de inclusión/exclusión, vinculados a una versión del catálogo.

### TST-018 --- Correspondencia entre contenido y PDA

-   **Categoría:** relaciones curriculares; ADR-011, criterio 3.
-   **Estado:** PENDING
-   **Objetivo:** comprobar relaciones explícitas grado/campo/contenido/PDA.
-   **Procedimiento propuesto:** cotejar cada relación del catálogo con
    la fuente de quinto; preparar un caso negativo con un PDA asociado
    a un contenido distinto del que le corresponde.
-   **Criterios:** las relaciones corresponden a la fuente; asociaciones
    inventadas, incompletas o cambiadas de campo no se marcan verificadas.
-   **Evidencia esperada:** reporte de correspondencias y discrepancias,
    con referencias de página y versión del catálogo revisado.

### TST-019 --- Fuente, edición, página y versión del catálogo

-   **Categoría:** trazabilidad curricular; ADR-011, criterio 4.
-   **Estado:** PENDING
-   **Objetivo:** poder reconstruir el origen de cada referencia usada.
-   **Procedimiento propuesto:** revisar los metadatos de registros y
    referencias de una planeación; contrastar edición y página con la
    fuente oficial e identificar la versión del catálogo utilizada.
-   **Criterios:** cada referencia conserva fuente, edición y página
    verificables, junto con versión del catálogo; los metadatos faltantes
    o discordantes se detectan. Una revisión nueva del catálogo no debe
    atribuirse silenciosamente a una planeación de una versión anterior.
-   **Evidencia esperada:** trazabilidad planeación/referencia/registro/
    fuente y reporte de casos con metadatos ausentes o inconsistentes.

### TST-020 --- Evaluación con docentes de quinto

-   **Categoría:** validación docente; ADR-011, criterio 5.
-   **Estado:** PENDING
-   **Objetivo:** evaluar pertinencia de las referencias y planeaciones
    con docentes de quinto, usando la rúbrica que se acuerde en M2.
-   **Procedimiento propuesto:** revisar casos representativos de los
    cuatro campos, registrar correcciones, discrepancias y valoración
    curricular y pedagógica por separado de conectividad y formato JSON.
-   **Criterios:** revisión docente documentada contra la fuente y la
    rúbrica; no aceptar una planeación solo por su extensión o HTTP 200.
    Acordar selección de casos y criterios de aprobación antes de ejecutar.
-   **Evidencia esperada:** rúbrica, versión de casos/catálogo, modelo y
    configuración cuando haya inferencia, observaciones y decisión de revisión.

### TST-021 --- Referencias generadas existentes en el catálogo

-   **Categoría:** validación de salida; ADR-011, criterio 6.
-   **Estado:** PENDING
-   **Objetivo:** comprobar que las referencias presentadas como
    verificadas corresponden a registros de la versión del catálogo usada.
-   **Procedimiento propuesto:** contrastar las referencias de salidas
    válidas y de casos con referencias inexistentes, de otra versión o grado.
-   **Criterios:** solo se presentan como verificadas las referencias
    existentes y pertinentes a quinto; las inexistentes o discordantes
    se detectan y no se atribuyen a SEP como válidas.
-   **Evidencia esperada:** correspondencia salida/registro/versión y
    resultados de los casos negativos, sin depender de una llamada pagada.

### TST-022 --- Control documental de derechos de reutilización

-   **Categoría:** condición documental independiente; ADR-011, criterio 7.
-   **Estado:** PENDING
-   **Objetivo:** resolver las condiciones aplicables antes de distribuir
    contenido protegido en un producto comercial.
-   **Procedimiento propuesto:** obtener revisión documentada de la
    fuente y edición exactas, uso previsto, alcance de reproducción y,
    cuando corresponda, autorización de sus titulares por el responsable.
-   **Criterios:** la conclusión y sus condiciones quedan respaldadas
    por evidencia revisada para el uso previsto. Si los permisos o su
    alcance siguen inciertos, el requisito permanece pendiente y no se
    presupone autorizada la distribución comercial del contenido.
-   **Límite:** una URL pública, un catálogo técnicamente correcto o una
    inferencia exitosa no acreditan derechos. Este control no emite por
    sí mismo autorización jurídica ni sustituye la revisión correspondiente.
-   **Evidencia esperada:** referencia a revisión y autorizaciones
    aplicables, fecha, fuente/edición, condiciones y límites documentados.

### TST-023 --- Presentación de referencias y sugerencias generadas

-   **Categoría:** uso curricular responsable; principios de ADR-011.
-   **Estado:** PENDING
-   **Objetivo:** distinguir contenido oficial verificable de sugerencias
    pedagógicas generadas y preservar el criterio profesional docente.
-   **Procedimiento propuesto:** revisar salidas y su presentación con
    referencias válidas, sin referencias verificables y con sugerencias
    de actividades o secuencias no establecidas por la fuente.
-   **Criterios:** no atribuir sugerencias ni secuencias inventadas a la
    fuente oficial; no presentar la planeación como sustituto del Programa
    Analítico escolar; permitir contextualización, adaptación y revisión
    docente. Una referencia no comprobada no se etiqueta como alineación
    curricular verificada.
-   **Evidencia esperada:** ejemplos sanitizados de presentación y reporte
    de revisión docente, con referencias al catálogo cuando proceda.

### 5.1 Matriz de trazabilidad de ADR-011 (validación pendiente)

| Cláusula de ADR-011 | Requisito/deuda | Documentos que la recogen | Prueba propuesta |
|---|---|---|---|
| Decisión 1–5 y validación 1: quinto México, catálogo y cuatro campos | RF-009, RNF-010, TD-007 | Índice ADR, arquitectura: alcance curricular; requisitos: alcance/RF-009; roadmap M2; AGENTS 3.4 | TST-016 |
| Principios y validación 2: separar sexto de quinto | RF-009, TD-007 | Arquitectura: alcance curricular; requisitos RF-009; roadmap M2; AGENTS 3.4 | TST-017 |
| Estrategia y validación 3: relaciones contenido/PDA | RF-009, RNF-010, TD-007 | Arquitectura: alcance curricular; requisitos RF-009; roadmap M2; AGENTS 3.4 | TST-018 |
| Principios y validación 4: fuente/edición/página y catálogo versionado | RF-009, TD-007 | Arquitectura: alcance curricular; requisitos RF-009; roadmap M2/M3; AGENTS 3.4 | TST-019 |
| Validación 5: evaluación con docentes de quinto | RF-009, RNF-010, TD-007 | Arquitectura: alcance curricular; requisitos RF-009; roadmap M2; AGENTS 3.4 | TST-020 |
| Validación 6: referencias utilizadas existentes | RF-009, RNF-010, TD-007 | Arquitectura: alcance curricular; requisitos RF-009; roadmap M2/M3; AGENTS 3.4 | TST-021 |
| Derechos y validación 7: reutilización pendiente | RF-009, TD-007 | Índice ADR, arquitectura: alcance curricular; requisitos: alcance/RF-009; roadmap M2; AGENTS 3.4 | TST-022, control documental separado |
| Principios: revisión, sugerencias, Programa Analítico y secuencias | RF-009, RNF-010, TD-007 | Requisitos RF-009; roadmap: docente en control; AGENTS 3.4 | TST-023 |
| Decisión: sin RAG/embeddings/base vectorial en el MVP; almacenamiento abierto | Restricción de ADR-011 sobre RF-009 | Arquitectura: alcance curricular; requisitos: alcance; roadmap: alcance/exclusiones; TD-007; AGENTS 3.4 | Revisión de coherencia documental; no acredita arquitectura implementada |

Esta matriz verifica cobertura de criterios en los documentos; no cambia los
estados PENDING ni cierra TD-007. La ejecución futura debe identificar commit,
versión del catálogo y evidencia según la plantilla de la sección 7.

## 6. Matriz mínima para comparar modelos

Cuando se comparen NVIDIA NIM, OpenAI u otros proveedores, utilizar
exactamente los mismos casos de entrada y la misma rúbrica.

  -----------------------------------------------------------------------
  Dimensión                           Qué registrar
  ----------------------------------- -----------------------------------
  Identificación                      Proveedor, modelo, versión/fecha si
                                      está disponible

  Entrada                             Caso de prueba y versión del prompt

  Resultado                           Respuesta completa o referencia
                                      segura al artefacto

  Calidad                             Puntuación por dimensión de la
                                      rúbrica

  Latencia                            Tiempo total y, si se puede, tiempo
                                      hasta primer token

  Consumo                             Tokens de entrada/salida reportados
                                      por el proveedor

  Costo                               Estimación con tarifa y fecha de
                                      referencia; indicar si no se conoce

  Fiabilidad                          Éxito, timeout, error HTTP, parsing
                                      o validación

  Revisión humana                     Evaluador, observaciones y decisión
  -----------------------------------------------------------------------

No comparar modelos con prompts, entradas, parámetros o criterios
distintos sin registrar esas diferencias. Repetir cada caso cuando se
necesite medir variabilidad; una sola respuesta no es una muestra
suficiente.

## 7. Plantilla para registrar una nueva ejecución

Copiar este bloque por cada ejecución relevante:

``` text
ID de prueba:
Fecha y hora:
Commit:
Entorno:
Proveedor:
Modelo:
Versión/configuración del prompt:
Datos de entrada:
Resultado: PASS / FAIL / INTERMITTENT / NEEDS REVIEW
Latencia:
Tokens de entrada:
Tokens de salida:
Costo estimado y tarifa usada:
Evidencia (captura, log sanitizado o artefacto):
Incidencias:
Evaluador:
Observaciones:
```

No adjuntar API keys, encabezados `Authorization`, cookies, datos
personales de estudiantes ni información sensible en evidencias.

## 8. Reglas de mantenimiento

1.  Actualizar el estado solamente después de ejecutar la prueba y
    conservar evidencia suficiente.
2.  Asociar cada ejecución con un commit y configuración identificables.
3.  Mantener separadas las pruebas de conectividad, integración,
    interfaz y calidad pedagógica.
4.  Si cambia el contrato de la planeación, actualizar los casos
    afectados.
5.  Si cambia el proveedor o modelo, conservar la ejecución anterior
    para comparación.
6.  No marcar una prueba como PASS únicamente porque la API respondió
    HTTP 200.
7.  Convertir defectos reproducibles en issues y relacionarlos con el
    identificador de deuda técnica o ADR correspondiente.

## 9. Documentos relacionados

-   `docs/architecture.md`
-   `docs/requirements.md`
-   `docs/roadmap.md`
-   `docs/technical-debt.md`
-   `docs/adr/`
-   `docs/research/`
