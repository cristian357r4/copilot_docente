# Roadmap --- Copilot Docente

> **Estado:** borrador de planificación\
> **Última actualización:** 26 de noviembre de 2026\
> **Versión de referencia:** `v0.1.0`\
> **Propósito:** ordenar la validación del producto y la evolución
> técnica sin convertir propuestas todavía no aprobadas en compromisos
> de implementación.

## 1. Dirección del producto

Copilot Docente busca ayudar al personal docente a preparar, revisar y
adaptar planeaciones didácticas. La herramienta debe funcionar como
asistente: el docente conserva el criterio profesional, revisa el
contenido generado y decide qué utilizar.

La prioridad inmediata no es construir una plataforma completa ni
introducir agentes autónomos. Es comprobar que el problema existe, que
el flujo de generación es útil para docentes reales y que la calidad del
resultado justifica continuar invirtiendo.

### Principios de trabajo

1.  **Docente en control:** la generación es un borrador editable, no
    una decisión pedagógica automática.
2.  **Calidad antes que cantidad:** una planeación extensa no es
    necesariamente una buena planeación.
3.  **Independencia del proveedor:** la lógica del producto no debe
    quedar acoplada a un proveedor o modelo concreto.
4.  **Inferencia cloud en el producto:** los modelos locales se reservan
    para desarrollo y experimentación; no se exige hardware
    especializado al docente.
5.  **Recuperación y control de costos:** las operaciones costosas deben
    tener estados, límites y comportamiento definido ante desconexiones
    o reintentos.
6.  **Fuentes verificables:** cualquier afirmación de alineación
    curricular deberá poder contrastarse con fuentes identificadas y
    vigentes.
7.  **Decisiones trazables:** las decisiones aprobadas se registran en
    `docs/adr/`; las propuestas siguen marcadas como propuestas.

El alcance curricular aceptado en [ADR-011](adr/0011-curriculum-knowledge.md)
es exclusivamente quinto de primaria en México. El catálogo estructurado y
versionado sigue pendiente de construcción y validación; los derechos de
reutilización también están pendientes. La exploración de otros niveles en
entrevistas no amplía el MVP. RAG, embeddings y bases vectoriales quedan fuera.

## 2. Convenciones de estado

-   **Completado:** existe evidencia verificable en el repositorio o en
    una prueba ejecutada.
-   **En curso:** trabajo iniciado, todavía sin cumplir todos los
    criterios de salida.
-   **Siguiente:** prioridad sugerida para la próxima iteración.
-   **Propuesto:** opción que requiere investigación o aprobación.
-   **Bloqueado:** depende de una decisión, acceso o validación
    pendiente.

Este roadmap es una guía de secuenciación. No sustituye los requisitos
(`docs/requirements.md`), las decisiones arquitectónicas
(`docs/architecture.md` y `docs/adr/`) ni el backlog de trabajo.

## 3. Hitos

### M0 --- Línea base técnica `v0.1.0`

**Estado: Completado**

Se estableció una primera línea base funcional con integración NVIDIA
NIM.

Evidencia registrada:

-   La compilación de producción (`npm run build`) terminó
    correctamente.
-   El servidor de Next.js inició localmente.
-   El endpoint de NVIDIA respondió y el catálogo devolvió el modelo
    configurado.
-   Se ejecutó una inferencia directa con NVIDIA NIM.
-   Se generó y visualizó una planeación de prueba desde la aplicación.
-   Los cambios se integraron mediante Pull Request #1.
-   Se publicó la etiqueta anotada `v0.1.0`.

**Alcance de lo demostrado:** el flujo básico funciona con el caso de
prueba realizado. Esto no demuestra todavía calidad pedagógica general,
estabilidad bajo carga, disponibilidad continua ni preparación para
producción.

**Pendientes conocidos:** validar OpenAI desde el flujo de la
aplicación; investigar el error DNS intermitente `ENOTFOUND`; ampliar
pruebas; validar calidad con docentes.

### M1 --- Fiabilidad del flujo de generación y proveedores

**Estado: Siguiente**

Objetivo: que el flujo de generación sea predecible, diagnosticable y
seguro antes de ampliar funcionalidades.

Líneas de trabajo:

-   Verificar la configuración y el flujo completo de cada proveedor
    soportado, comenzando por OpenAI y NVIDIA NIM.
-   Mantener la selección de proveedor y modelo configurable mediante
    configuración de entorno, sin dispersar detalles del proveedor por
    la interfaz.
-   Definir un contrato común para solicitud, respuesta, errores,
    límites y metadatos de uso.
-   Diferenciar errores de DNS, conexión, timeout, autenticación, cuota,
    respuesta inválida y error del proveedor.
-   Definir políticas explícitas de timeout, reintentos y cancelación;
    evitar reintentos automáticos que puedan duplicar inferencias
    facturables.
-   Validar la respuesta estructuralmente antes de presentarla como una
    planeación.
-   Evitar exponer claves, encabezados de autorización, prompts
    sensibles o datos personales en logs.
-   Documentar los estados que verá el usuario cuando una generación
    tarde, falle o pueda recuperarse.

**Criterios de salida sugeridos:**

-   Existe al menos una prueba de integración reproducible por proveedor
    habilitado.
-   Los errores se clasifican y muestran sin revelar secretos.
-   Las respuestas que no cumplen el contrato no se presentan como
    resultados correctos.
-   Se conoce qué sucede si el usuario actualiza, cierra o vuelve a
    abrir la página durante una generación.
-   Se puede identificar el consumo por solicitud cuando el proveedor lo
    informa.

**No decidir todavía:** tecnología de cola, base de datos, gateway de
modelos o estrategia final de persistencia. Registrar alternativas y
justificar la elección mediante ADR cuando exista suficiente evidencia.

### M2 --- Descubrimiento docente y calidad pedagógica

**Estado: Siguiente, en paralelo con M1**

Objetivo: comprobar que la herramienta resuelve un problema real del
trabajo docente y definir cómo evaluar una planeación útil.

Actividades:

1.  Entrevistar a docentes de distintos niveles y contextos. Investigar
    cómo preparan hoy sus planeaciones, cuánto tiempo invierten, qué
    formatos utilizan y qué partes les resultan más difíciles.
2.  Observar o reconstruir un flujo real de preparación, revisión y
    adaptación de una planeación.
3.  Recopilar ejemplos anonimizados o autorizados de planeaciones y
    formatos de referencia.
4.  Identificar las diferencias por nivel educativo, grado, asignatura,
    duración, contexto escolar y recursos disponibles.
5.  Definir una rúbrica de evaluación con docentes antes de comparar
    modelos.
6.  Identificar la fuente curricular oficial de SEP para quinto de
    primaria, verificando la referencia y edición registradas en ADR-011,
    fecha, páginas y condiciones de reutilización. Resolver la revisión
    de derechos y, cuando corresponda, la autorización antes de distribuir
    contenido protegido en un producto comercial.
7.  Construir un catálogo estructurado y versionado de los contenidos y
    PDA de quinto de primaria en los cuatro campos formativos, con
    relaciones explícitas y trazabilidad a la fuente. Excluir contenidos
    y PDA exclusivos de sexto; el almacenamiento físico sigue abierto.
8.  Validar la integridad del catálogo y la correspondencia entre sus
    registros, la fuente oficial y las referencias incluidas en las
    planeaciones, con docentes de quinto grado.

**Dimensiones iniciales para la rúbrica** --- sujetas a validación
docente:

-   Correspondencia entre contenido, propósito u objetivo y actividades.
-   Secuencia didáctica y coherencia entre inicio, desarrollo y cierre.
-   Adecuación al nivel, grado y tiempo disponible.
-   Evaluación alineada con los aprendizajes esperados o propósitos
    definidos.
-   Viabilidad con los recursos y condiciones del grupo.
-   Claridad, aplicabilidad y posibilidad de adaptación.
-   Exactitud disciplinar y ausencia de instrucciones contradictorias.

**Criterios de salida sugeridos:**

-   Se documentan los principales problemas y necesidades observados.
-   Existe una rúbrica revisada por docentes.
-   Hay un conjunto de casos de prueba representativos y autorizados.
-   Se distingue explícitamente entre alineación curricular comprobada y
    contenido generado sin fuente verificable.
-   El catálogo de quinto de primaria conserva versión, fuente, edición
    y página para cada referencia utilizada.
-   Se verifica la cobertura de Lenguajes; Saberes y Pensamiento
    Científico; Ética, Naturaleza y Sociedades; y De lo Humano y lo
    Comunitario, sin incorporar contenidos o PDA exclusivos de sexto.
-   La validación curricular se registra mediante TST-016 a TST-021 y
    TST-023; la condición documental de derechos se revisa por separado
    en TST-022. Todos están PENDING en [test-plan.md](testing/test-plan.md).

No afirmar alineación con el plan o programa oficial únicamente porque
el modelo mencione términos curriculares.

### M3 --- MVP centrado en el flujo docente

**Estado: Propuesto; depende de M2**

Objetivo: convertir la demostración de generación en un flujo que el
docente pueda utilizar, revisar y continuar.

Capacidades candidatas:

-   Capturar los datos necesarios para contextualizar una planeación.
-   Generar un borrador con estructura consistente.
-   Permitir revisar y editar el resultado antes de utilizarlo.
-   Conservar el trabajo del usuario y permitir consultarlo
    posteriormente.
-   Indicar proveedor/modelo y estado de generación cuando sea útil.
-   Permitir exportar o compartir el resultado, después de validar los
    formatos que realmente utilizan los docentes.
-   Mostrar limitaciones y advertir cuando no se haya verificado una
    referencia curricular.
-   Consultar el catálogo curricular estructurado y versionado de quinto
    de primaria al incluir referencias verificadas en una planeación,
    usando la versión validada y respetando las condiciones de
    reutilización documentadas; no atribuir alineación antes de comprobarla.

**Criterios de salida sugeridos:**

-   Un docente puede completar el flujo principal sin asistencia del
    desarrollador.
-   Puede corregir el resultado y conservar su versión revisada.
-   La aplicación no pierde silenciosamente un trabajo ante una
    desconexión o recarga.
-   Los casos de prueba definidos en M2 se ejecutan y sus resultados
    quedan registrados.

La selección de formatos de exportación, historial, plantillas y otras
capacidades deberá priorizarse con evidencia de uso, no por conveniencia
técnica.

### M4 --- Persistencia, seguridad y operación

**Estado: Propuesto; depende de los requisitos de M1 y M3**

Objetivo: preparar la aplicación para un piloto controlado y,
posteriormente, para un servicio operado de forma confiable.

Áreas por resolver:

-   Identidad, autenticación, autorización y separación de datos entre
    usuarios u organizaciones.
-   Persistencia de solicitudes, estados, resultados y errores.
-   Idempotencia para evitar que reintentos del cliente creen trabajos
    duplicados.
-   Recuperación de trabajos tras desconexiones o reinicios.
-   Retención, exportación y eliminación de datos.
-   Protección de credenciales y gestión de secretos.
-   Límites por usuario u organización, cuotas y presupuestos.
-   Observabilidad de latencia, errores, tokens y costos.
-   Copias de seguridad, restauración y estrategia de despliegue.

**Criterio de salida:** existe un modelo operativo documentado y probado
para el alcance concreto del piloto. No introducir infraestructura
distribuida si una solución más sencilla satisface los requisitos
medidos.

### M5 --- Piloto con docentes

**Estado: Propuesto; depende de M2, M3 y de las salvaguardas mínimas de
M4**

Objetivo: comprobar utilidad, calidad y comportamiento en situaciones
reales, con un grupo pequeño y voluntario.

Diseño sugerido:

-   Seleccionar docentes y escenarios representativos.
-   Explicar que el resultado es un borrador que requiere revisión
    profesional.
-   Recoger ejemplos de entradas, resultados, ediciones y motivos de
    rechazo, con consentimiento y minimización de datos.
-   Comparar el proceso habitual con el flujo asistido, sin asumir que
    toda reducción de tiempo representa una mejora.
-   Registrar incidentes, errores factuales, problemas curriculares y
    resultados no utilizables.

**Métricas candidatas:**

-   Tiempo hasta obtener un primer borrador útil.
-   Porcentaje de resultados aceptados con cambios menores, modificados
    sustancialmente o descartados.
-   Tipos y frecuencia de errores pedagógicos o disciplinares.
-   Satisfacción y disposición a volver a utilizarlo.
-   Costo de inferencia por resultado útil, no solamente por llamada.
-   Frecuencia de fallos, reintentos y trabajos recuperados
    correctamente.

**Criterio de salida:** evidencia suficiente para decidir si se itera,
se cambia el alcance o se detiene el producto. No fijar metas numéricas
sin una línea base y sin acordarlas con los participantes.

### M6 --- Modelo operativo y viabilidad comercial

**Estado: En investigación; depende de evidencia del piloto**

Comparar alternativas sin asumir de antemano que una será la definitiva:

-   **SaaS administrado:** el producto opera la aplicación y
    contrata/provee el acceso a modelos; requiere analizar costos,
    facturación, soporte, privacidad y márgenes.
-   **BYOK:** el cliente aporta su propia clave o cuenta de proveedor;
    requiere estudiar experiencia de configuración, soporte, límites,
    seguridad y responsabilidades de pago.
-   **Self-hosted institucional:** una organización instala o administra
    parte de la solución; requiere evaluar capacidad técnica,
    actualizaciones, soporte y operación.
-   **Modelo híbrido:** distintas modalidades según tipo de cliente,
    solamente si la complejidad adicional se justifica.

Preguntas que deben resolverse con investigación:

-   ¿Quién es el comprador: docente individual, escuela, red escolar o
    institución?
-   ¿Quién autoriza y paga?
-   ¿Qué problema tiene suficiente valor para justificar un pago
    recurrente?
-   ¿Qué costos variables y fijos genera el servicio?
-   ¿Qué responsabilidades de soporte y protección de datos asume el
    proyecto?
-   ¿BYOK reduce realmente el costo operativo o traslada demasiada
    complejidad al docente?
-   ¿Qué condiciones de conectividad, dispositivos y administración
    existen en las escuelas objetivo?

**Criterio de salida:** comparación documentada con evidencia de
entrevistas, costos y pruebas. No implementar facturación ni onboarding
BYOK completo antes de validar el segmento y la disposición de pago.

## 4. Secuencia y dependencias

La secuencia recomendada es:

1.  Mantener `v0.1.0` como punto de recuperación.
2.  Trabajar M1 y M2 en paralelo: fiabilidad técnica y descubrimiento
    docente.
3.  Ajustar el alcance de M3 con los hallazgos de M2.
4.  Implementar en M4 únicamente la infraestructura necesaria para
    soportar el flujo y el piloto.
5.  Ejecutar M5 con criterios de evaluación previamente acordados.
6.  Utilizar la evidencia de M5 para decidir M6.

M3, M4, M5 y M6 no deben tratarse como compromisos de alcance cerrado
hasta superar sus dependencias.

## 5. Fuera de alcance por ahora

No priorizar hasta contar con evidencia que lo justifique:

-   Agentes autónomos que ejecuten acciones sin revisión docente.
-   Arquitectura multiagente.
-   Despliegue de modelos locales en los equipos de los docentes.
-   Self-hosting generalizado para usuarios individuales.
-   Facturación, planes comerciales y marketplace.
-   Integraciones amplias con LMS o sistemas escolares.
-   RAG, embeddings y bases vectoriales para el catálogo curricular del
    MVP; reconsiderarlos solo ante una necesidad demostrada.
-   Infraestructura de colas o microservicios por anticipado.

Estas ideas no quedan descartadas; quedan aplazadas para evitar
complejidad prematura.

## 6. Riesgos y preguntas abiertas

  -----------------------------------------------------------------------
  Riesgo o pregunta                   Tratamiento
  ----------------------------------- -----------------------------------
  El modelo produce texto convincente Rúbrica, revisión docente y casos
  pero incorrecto                     de prueba

  La planeación no corresponde al     Descubrimiento y captura de
  contexto escolar                    contexto

  Cambios curriculares vuelven        Identificar fuente, versión y fecha
  obsoletas las referencias

  Fallos de red generan solicitudes   Estados, idempotencia y
  repetidas                           recuperación

  Costos crecen sin control           Medición, límites y presupuestos

  BYOK aumenta la carga de soporte    Probar comprensión y configuración
                                      con usuarios

  Datos de alumnos o docentes quedan  Minimización, privacidad, retención
  expuestos                           y controles

  La aplicación se vuelve compleja    Hitos con criterios de salida y
  antes de validar valor              alcance limitado
  -----------------------------------------------------------------------

Preguntas pendientes:

-   ¿Qué formato de planeación utiliza cada segmento docente?
-   ¿Cómo verificar la integridad y vigencia de la fuente de quinto
    registrada en ADR-011 y resolver sus derechos de reutilización?
-   ¿Qué información es indispensable y cuál agrega fricción al
    formulario?
-   ¿Qué datos deben persistirse y durante cuánto tiempo?
-   ¿Qué nivel de historial y recuperación esperan los docentes?
-   ¿Qué proveedor ofrece el equilibrio adecuado entre calidad,
    latencia, disponibilidad y costo?
-   ¿Quién pagaría por el producto y bajo qué modalidad?

## 7. Relación con otros documentos

-   `docs/architecture.md`: arquitectura y contexto técnico.
-   `docs/requirements.md`: requisitos y criterios de aceptación.
-   `docs/adr/`: decisiones y alternativas arquitectónicas.
-   `docs/research/`: entrevistas, comparativas, fuentes y experimentos.
-   `docs/testing/`: casos de prueba, rúbricas y resultados.
-   `docs/technical-debt.md`: problemas conocidos y deuda técnica.

Cuando un hito se divida en trabajo ejecutable, crear GitHub Issues con
alcance, prioridad, dependencias y criterios de aceptación. No convertir
automáticamente cada línea de este roadmap en una tarea.

## 8. Regla de mantenimiento

Actualizar este documento cuando:

-   un hito cambie de estado;
-   una prueba o entrevista aporte evidencia que cambie prioridades;
-   una decisión arquitectónica sea aceptada, rechazada o sustituida;
-   cambie el alcance del producto.

Registrar la fecha y enlazar la evidencia o ADR correspondiente.
Mantener las hipótesis identificadas como tales y no presentar
propuestas como decisiones aprobadas.
