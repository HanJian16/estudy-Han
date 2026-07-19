# Versiones de la arquitectura

> **Qué es esto.** El registro de **qué tiene el proyecto en cada versión**: lo que se añadió, lo que cambió y lo que se eliminó. Responde *"¿qué hay ahora y desde cuándo?"*.
>
> **Qué NO es.** No es la lista de errores — esa es [`ERRORES.md`](ERRORES.md), organizada por patrón de fallo. Cada versión de aquí **enlaza** a los patrones que cerró, pero no los describe: describirlos en dos sitios es garantizar que uno se quede viejo.
>
> **Por qué existe pese a que hay git.** Porque **nada invoca a git**. Nunca se ha revisado el historial de commits para responder una pregunta del proyecto, y no hay ningún disparador que mande hacerlo — es el patrón P1 de `ERRORES.md`, y este archivo nació justamente de haberlo cometido al argumentar que "git ya lo cubre". Git guarda el grano fino (el diff por commit) y sigue siendo la fuente para eso; este archivo guarda el grano que se usa al decidir.

## INVOCADORES

**De ESCRITURA (el que mantiene vivo el archivo — el importante):**
**Todo cambio de Capa 2 termina escribiendo aquí su entrada**, en el mismo turno en que se hace el cambio. Está cableado como último item de las Ramas 2A/2B de [`_protocolos/capas.md`](_protocolos/capas.md). Si el cambio además cerró un error, se documenta en `ERRORES.md` y aquí solo se enlaza.

**De LECTURA:**
- Cuando el usuario pregunta qué cambió, en qué versión está, o cuándo entró algo.
- Cuando algo se comporta distinto de lo esperado y hay que saber qué se movió por última vez.

**Numeración:** sube la **mayor** (v3 → v4) cuando cambia el modelo conceptual —algo que obliga a releer el README para entenderlo—; la **menor** (v3 → v3.1) para añadidos y arreglos que encajan en el modelo vigente.

## MARCADOR `[SIM:]` — obligatorio en cada viñeta

Toda viñeta de **Añadido** y **Cambiado** termina con uno de estos dos, **literalmente**:

```
[SIM: 2026-07-18]                      ← se corrió el simulacro de la Rama 2C, ese día
[SIM: no aplica — <razón concreta>]    ← no había nada que ejecutar, y se dice por qué
```

**Para qué sirve:** la **alerta 18** (BARATA, corre en cada arranque) cuenta las viñetas de la versión más reciente **sin marcador** y las saca a la superficie. Es el verificador de la Rama 2C, y **es lo único del ciclo que no depende de la memoria de nadie.**

Nació el 2026-07-18: la Rama 2C se saltó cuatro veces en un día pese a estar escrita, y el primer intento de arreglo —exigir que el agente enseñara la tabla del simulacro— dejaba el verificador en la atención del usuario, que tendría que recordar durante meses que esas tablas deben aparecer. **Un control cuyo verificador es la memoria humana no es un control.** El marcador mueve la prueba al repo.

`[SIM: no aplica]` **sin razón escrita cuenta como no puesto.** Es la vía de escape obvia y por eso está cerrada.

---

## v3.2 — El plan del motor · 2026-07-19

> Sesión de consulta con Fable sobre si "migrar los protocolos a código" era el mejor camino. Veredicto: la dirección es correcta, pero al plan le faltaban los **hooks de Claude Code** (SessionStart/PostToolUse — corren en la máquina, fuera del agente, en cada sesión, no solo al commitear) y le sobraba alcance ("migrar todo" → núcleo de ~6 módulos + poda de prosa). Salió el paso a paso ejecutable completo, pensado para que lo ejecute Sonnet 5/Opus 4.8 un paso por sesión sin contexto previo.

**Añadido**
- **`PLAN-MOTOR.md`**: las 5 fases de la migración (verificador+hooks, alertas, grafo/selector/viabilidad, poda, backlog) con reglas de ejecución, criterios de aceptación por comando, tests de regresión atados a las cifras reales documentadas (49 temas base, 65 del parcial, ~215 h, déficit ~44 h) y modelo recomendado por paso. Detalla la Prioridad 1 de `PENDIENTE.md` sin duplicar su porqué. [SIM: 2026-07-19]

**Cambiado**
- **`PENDIENTE.md`** apunta al plan; matiza el "después del semestre" (Fases 0–2 adelantables en huecos) y marca el hook de git como PLANIFICADO (paso 1.3 del plan). [SIM: no aplica — punteros y estado, sin lógica que ejecutar]
- **`CLAUDE.md`** corregido: decía que `PENDIENTE.md` quedaba "por una sola cosa" (el porteo) cuando contenía la Prioridad 1 entera desde el 18-jul. [SIM: no aplica — corrección de texto; el error está documentado como caso nuevo del patrón de arrastre]

**Errores:** nueva instancia del patrón **P9** en `ERRORES.md` (el puntero de `CLAUDE.md` quedó viejo al modificar `PENDIENTE.md`; lo cazó la revisión completa, no un mecanismo del proyecto). Y el simulacro del propio plan cazó dos fallos del plan antes de darse por hecho: una cifra en prosa equivocada (150 temas donde son 160) y un falso positivo del futuro detector de punteros (ejemplos entre backticks) — ambos corregidos en el mismo turno.

---

## v3.1 — El verificador · 2026-07-18

> v3 construyó el ciclo; v3.1 lo hace **comprobable sin depender de la memoria de nadie**. Salió entera de que el usuario rechazara el arreglo anterior: *"si el detector es que yo note que falta la tabla, el verificador es mi memoria — y el proyecto existe porque la memoria es frágil."*

**Añadido**
- **Regla anti-jerga** en las reglas de comportamiento del `README.md`: prohibido citar un identificador interno (número de alerta, patrón, rama, paso, id de objetivo, nombre de campo) sin decir qué es en la misma frase. Con prueba práctica: si al borrar el identificador la frase deja de entenderse, la frase estaba mal. Extiende a todo el vocabulario la regla que ya existía solo para los objetivos. [SIM: 2026-07-18]

**Cambiado**
- **Marcador `[SIM:]` obligatorio** en cada viñeta de `VERSIONES.md`, con especimen literal y la vía de escape cerrada (`no aplica` sin razón = no puesto). [SIM: 2026-07-18]
- **Alerta 18** (BARATA): cuenta las viñetas de la versión más reciente sin marcador y las reporta en cada arranque. Es el verificador de la Rama 2C, y lo único del ciclo que no descansa en que alguien se acuerde. [SIM: 2026-07-18]

- **La Rama 2A lee LOS DOS archivos de memoria, siempre** (`ERRORES.md` + `VERSIONES.md`), al crear y al modificar. Antes el reparto era artificial: patrones solo al crear, versiones solo al modificar. Al crear también importa si ya se construyó algo parecido; al modificar también importan las trampas. [SIM: 2026-07-18]
- **Tabla del ciclo cerrado**: cuarta fila con las dos comprobaciones automáticas (errores sin analizar y cambios sin probar) como verificación externa, y nota de que el ciclo **no es secuencial**. [SIM: 2026-07-18]
- **Corregida incoherencia** en la cabecera de `ERRORES.md`: decía "se lee en dos momentos" y ya listaba tres. [SIM: no aplica — corrección de texto, no hay lógica que ejecutar]

**Errores cerrados:** una instancia más de **P1** — un control cuyo verificador es la atención del usuario es un invocador manual disfrazado.

---

## v3 — Cableado y árbol real · 2026-07-18

> El modelo de v2 era correcto y estaba **apagado**: sus detectores dependían de que el usuario escribiera un comando a mano. v3 no cambia el modelo, cambia **quién lo dispara**. Toda ella salió de una sola sesión de auditoría; el usuario cazó cuatro de los errores.

> ⚠️ **DEUDA HONESTA DE ESTA VERSIÓN.** El marcador `[SIM:]` se inventó al final de esta misma sesión, así que **la mayoría de los cambios de v3 se escribieron SIN simulacro** — que es justamente el fallo que v3 documenta, cometido mientras se arreglaba. Marcarlos ahora con un `[SIM:]` retroactivo sería falsificar el registro. Van sin marcador **a propósito**: la alerta 18 los va a reportar, y eso es correcto — son deuda real. El convenio se aplica **desde v3.1 en adelante**, y esta lista queda como constancia de cuánto se escribió sin probar.

**Añadido**
- **PRE-FLIGHT** (regla 6 del PASO 5.5): corre **todas las alertas BARATAS en cada arranque**, automático. Sustituye al CANARIO, que solo cubría 2 de 16 condiciones.
- **Clasificación BARATAS / CARAS** del bloque de alertas, con default seguro: alerta sin coste declarado = BARATA.
- **`objetivo.json → ultima_auditoria`**: checkpoint fechado de la última cola de viabilidad. Único escritor: el comando `estado`. Campo `invalidada_por` para marcar cifras calculadas sobre un modelo que resultó falso.
- **Alertas 15** (auditoría vencida o invalidada) **y 16** (cadencia temporal vencida).
- **Auto-ejecución de la partida CARA** cuando la alerta 15 se cumple: se auto-limita a una vez cada ~21 días. `estado` a mano deja de ser la única vía para que las alertas caras existan.
- **Rama 2C de `capas.md` — simulacro obligatorio**: ninguna regla está terminada hasta ejecutarla contra el estado real.
- **PUERTA DE SALIDA de la Capa 2**: prohibido anunciar un cambio como hecho sin emitir la tabla del simulacro en el mismo mensaje, con **especimen literal** de la tabla. Nace de que la Rama 2C se saltó **cuatro veces el mismo día pese a estar escrita**: no dejaba rastro, así que un cambio simulado y uno sin simular se veían idénticos. Atar el control al **reporte** —lo único que el agente nunca omite— lo vuelve verificable por el usuario.
- **Prueba de sesión fría** como tercera comprobación del simulacro: releer lo escrito buscando cada punto donde un agente tendría que **inferir** qué se quiso decir. Cada inferencia es un error futuro.
- **Bandeja de entrada** en `ERRORES.md` + marca literal `[SIN ANALIZAR]`, para anotar en bruto un error detectado a mitad de clase sin romper el estudio.
- **Alerta 17**: cuenta las entradas de la bandeja y las saca a la superficie hasta procesarlas. Sin ella, "anótalo y sigue" era una nota que se pudre — P1 dentro del archivo que documenta P1.
- **Regla de PODA** en `ERRORES.md`: los patrones son permanentes, los casos no (>5 casos → deja 2 y resume).
- **El ciclo cerrado** documentado en `capas.md`: los cuatro invocadores (escribir error / leer patrones / leer versiones / escribir versión) en una tabla, para que se vea si alguno deja de dispararse.
- **Invocador de LECTURA de `VERSIONES.md`** en la Rama 2B: antes de modificar algo, mirar cuándo se tocó por última vez y por qué — evita revertir un arreglo reciente o re-arreglar lo ya arreglado. Faltaba: el archivo nació solo con invocador de escritura.
- **`ERRORES.md`**: catálogo por patrón + protocolo de documentación de errores, invocado desde Capa 1 y Capa 2A.
- **`VERSIONES.md`**: este archivo.
- **Conjunto `mantener()`** en el PASO 5.5, separado de `aprender()`.

**Cambiado**
- **El PASO 5.5 calcula sus candidatos con la CERRADURA**, no con `cursos_requeridos` a secas, reutilizando la `cerradura()` de la viabilidad.
- **La urgencia efectiva pasa a ser por TEMA**, no por curso.
- **La rama (F) comprueba `prerrequisitos_externos` al elegir**, no después.
- **La rama (C) opera sobre `mantener()`**, que no corta por nivel alcanzado.
- **`obj-1` (examen UNI) → `latente`**, con `condicion_activacion` atada a la `fecha_limite` de obj-2 (leída, no copiada). Decisión del usuario, con el coste aceptado por escrito: el examen de febrero puede no alcanzar.
- **Alerta 2** filtra por objetivo `activo` (no por `fecha_inicio`) y **agrega en una línea**.
- **Viabilidad recalculada** con la cerradura transitiva: parcial −44 h, final −62 h acumuladas. La cifra anterior (−52 h) contaba 27 temas base en vez de 49.
- **`_porteo-template.md`**: items 19 y 20, sin los cuales el template heredaría estos bugs.

**Eliminado**
- El bloque `_cadencia_temporal` de `07-aptitud-academica` (la cadencia relajada a 7 días duró unas horas): la latencia de obj-1 hace el mismo trabajo, y dos palancas para un fin se desincronizan.

**Errores cerrados:** P1, P2, P3, P4, P8, P9 de [`ERRORES.md`](ERRORES.md).

---

## v2 — Dos dimensiones · 2026-07-16 / 2026-07-17

> El rediseño del núcleo: el conocimiento deja de ser un porcentaje y pasa a tener dos coordenadas ortogonales.

**Añadido**
- **`nivel_alcanzado`** (profundidad: `null → base → intermedio → avanzado`), estado **verificable**: solo sube contra evidencia, nunca por gastar sesiones.
- **`nivel_repaso` + `fecha_proximo_repaso`** (durabilidad) y la **escalera de repaso adaptativa al hito**, atada a la profundidad y no a `estado`.
- **`historial.json`** append-only: la única fuente real sobre cuánto se estudia.
- **`veces_repasado`**: contador acumulativo que no se resetea al fallar.
- **Coste de MANTENER** en todas las validaciones: `horas_necesarias = horas_aprender + horas_mantener`.
- **Cola con prioridad** con `proyectado[]`: cada curso se cobra una sola vez.
- **Cerradura transitiva** parando en lo ya suficiente *para ese objetivo*.
- **`hitos`** por objetivo, con `hito_id` por tema y `condicional` para sustitutorios.
- **`rubrica_niveles`** por curso, expresada como **capacidades**, nunca como el puntaje del examen de un tercero.
- **Predicción de olvido** obligatoria.
- **`_protocolos/capas.md`** y **`_protocolos/coordinacion-agenda.md`** (loop dominical con Ángela).

**Cambiado**
- `cursos_requeridos` pasa a **muchos-a-muchos con `nivel_requerido` + `overrides`**; nace la **urgencia efectiva**.
- `presupuesto_horas` pasa de escalar a **array de periodos** que se integran.
- `un_tema_por_sesion` → **`una_clase_por_sesion`**.
- `HORAS_POR_SESION` y `minutos_por_repaso` **se miden**, y si no hay datos se declara que son suposiciones.

**Eliminado**
- `porcentaje`, `liston` (`dominar`/`pasar`), `sesiones_estimadas`, `pagado[]`/`ya_cobrados`, el "dueño" único de cada curso, `horas_semanales` por objetivo, `horas_semanales_disponibles` escalar.
- `"completado"` como **medida de conocimiento** (sobrevive solo como estado de **flujo**).

**Errores cerrados:** P4, P5, P6, P7.

---

## v1 — Modelo original · hasta 2026-07-15

Primer plan funcional: cursos con temario, progreso por porcentaje, un objetivo con una fecha, presupuesto de horas como número único, listón `dominar`/`pasar`. Cada curso tenía un dueño del que heredaba fecha y prioridad.

**Su fallo de diseño de fondo**, que v2 corrigió: medía **esfuerzo gastado** en vez de **estado verificado**, y respondía "¿está hecho?" cuando la pregunta útil era "¿hecho **para qué objetivo**?".
