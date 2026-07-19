# Protocolo de capas — qué releer antes de actuar

> **Por qué existe.** Este proyecto tiene mucha superficie de instrucciones. El modo de falla no es ignorarlas: es **creer que las recuerdas y actuar desde el recuerdo comprimido**. Ya pasó — se afirmó que unas fechas eran firmes teniendo abierto el archivo que decía lo contrario. La regla 3 del `README.md` ya resuelve esto para los DATOS ("SIEMPRE lee los JSON antes de dar una clase, no confíes en la memoria de la conversación"). Este archivo extiende esa misma disciplina a los PROTOCOLOS.
>
> **Este archivo tiene que ser barato de releer.** Un checklist caro se salta, y entonces es ceremonia sin seguridad — peor que nada. Si crece, pártelo: lo pesado va a las capas 2 y 3, que solo se jalan cuando la acción cae ahí.
>
> ---
>
> **SEGUNDO MODO DE FALLA — el que este archivo NO puede atrapar solo (añadido 2026-07-18).** Leer los protocolos de tu capa es necesario y **no es suficiente**. Caso real: el agente leyó este archivo entero y el `README.md` completo, clasificó bien la acción ("selector de clase" → Capa 3 → PASO 5.5), releyó el PASO 5.5 entero… y aun así falló. ¿Por qué? Porque **la regla que necesitaba vivía en el protocolo de OTRA acción** (el bloque de alertas de `utilidades.md`, alerta 9) y nada en el PASO 5.5 apuntaba hacia allá.
>
> Este archivo te protege de *"no leíste el protocolo de tu acción"*. **No te protege de *"el protocolo de tu acción está incompleto"***. Esa segunda falla no se arregla leyendo más: se arregla **cableando** —que la regla se invoque desde el momento en que hace falta— y por eso la Rama 2A ahora pregunta por el momento de uso y por si el invocador corre solo. Si al releer un protocolo notas que una regla que necesitas vive en otro sitio, **el arreglo no es acordarte mejor: es dejar el puntero escrito** para la sesión que viene.

---

## Cómo se usa, en cada mensaje

1. **Desglosa el mensaje en acciones discretas.** Los mensajes del usuario traen varias cosas en uno. Sin este paso se contesta la más ruidosa y las demás se caen en silencio.
2. **Verifica las restricciones vigentes** (abajo) antes de actuar.
3. **Clasifica cada acción por capa y relee los protocolos de esa capa antes de ejecutarla.**

**Las capas son ACUMULATIVAS, no excluyentes.** Si una acción cae en varias, corren todas. Clasificar de más nunca es el error caro; clasificar de menos sí.

---

## Capa 1 — SIEMPRE (toda acción, sin excepción)

Invariantes que no dependen de qué estés haciendo:

- **El estado vive en ARCHIVOS del repo**, no en tu memoria ni en la conversación. Si algo debe sobrevivir a la sesión, va a un archivo — el usuario puede cambiar de agente.
- **Reutiliza el procedimiento canónico; no improvises uno paralelo** (regla irrompible 6 del `README.md`).
- **Principios para decidir; literal para escribir.** La cláusula operativa se enuncia como principio general; un caso concreto solo aparece como ilustración marcada. Pero toda marca, plantilla o formato va con su especimen textual completo — describir un formato en prosa obliga a inferirlo, y la inferencia varía entre sesiones.
- **No afirmes lo que no verificaste.** Si es supuesto, dilo. Si el archivo está a mano, léelo antes de afirmar.
- **Nada que deba sobrevivir puede apuntar a algo irrecuperable** ("como acordamos en la conversación", "ver el chat"). Una sesión futura no puede abrirlo.
- **Ninguna regla sin invocador.** Una regla que nada dispara es decoración. Y desconfía de *"eso ya lo cubre X"*: si nada manda mirar X, no lo cubre.
- **Si aparece un ERROR del proyecto, se documenta ANTES de arreglarlo.** Da igual quién lo encuentre, en qué tipo de sesión, o si se arregla en el momento. Ve a [`ERRORES.md`](../ERRORES.md) → "Cómo se documenta un error nuevo" y sigue el procedimiento. **Se escribe antes porque mientras arreglas, la atención se va al arreglo y el error se da por sabido** — y si la sesión se corta a la mitad, se pierden los dos. Un error detectado y no escrito se repite: este proyecto ya repitió tres patrones distintos por eso.

---

## Capa 2 — cambias las REGLAS o la ESTRUCTURA del proyecto

Aplica cuando tocas un protocolo, el `README.md`, el esquema de datos, o creas un campo/archivo nuevo. **Antes de escribir, relee el archivo que vas a tocar.**

### El ciclo cerrado (para qué sirven `ERRORES.md` y `VERSIONES.md`)

Los dos archivos **no son un registro que se llena y nadie lee**. Cada uno tiene invocador de escritura *y* de lectura, y juntos cierran un ciclo. Si en algún momento uno de estos cuatro pasos deja de dispararse, los archivos se convierten en decoración — que es el patrón P1 y es exactamente cómo empezó todo esto:

| Cuándo | Qué se hace | Archivos | Invocador |
|---|---|---|---|
| **Se detecta un error** (cualquier sesión, lo cace quien lo cace) | Se **escribe** el caso, antes de arreglarlo | `ERRORES.md` | **Capa 1** — corre en cada mensaje |
| **Antes de crear o modificar** | Se **leen LOS DOS**: patrones en los que ya se cayó, y si ya se creó/tocó algo parecido y por qué | `ERRORES.md` **+** `VERSIONES.md` | **Rama 2A**, primer item (2B lo hereda) |
| **Al terminar el cambio** | Se **escribe** la entrada de versión **con su marcador `[SIM:]`** + se corre el simulacro y se emite su tabla | `VERSIONES.md` | **Cierre de Capa 2** |
| **En cada arranque, sin que nadie lo pida** | Se **comprueba** que no haya cambios sin simulacro ni errores sin analizar | — | **Alertas 18 y 17** (BARATAS, PRE-FLIGHT) |

**Los dos archivos se leen SIEMPRE JUNTOS, tanto al crear como al modificar.** Partirlos —patrones solo al crear, versiones solo al modificar— es un corte artificial: al crear algo nuevo también importa si ya se construyó algo parecido, y al modificar también importa en qué trampas se cayó la última vez.

**El ciclo no es estrictamente secuencial:** terminar un cambio suele destapar errores nuevos, y esos vuelven al primer paso en el acto — se escriben antes de arreglarlos, aunque estés a mitad de otra cosa.

**La última fila es la que sostiene todo.** Los tres primeros pasos dependen de que el agente los ejecute; **el cuarto los verifica desde fuera, en cada arranque, sin depender de la memoria de nadie** — ni la del agente ni la del usuario. Sin ella, este ciclo sería una buena intención documentada.

### Rama 2A — CREAR algo nuevo
- **LEE LOS DOS ARCHIVOS DE MEMORIA, SIEMPRE LOS DOS.** Van juntos y es el primer item a propósito: es más barato reconocer una trampa conocida que descubrirla otra vez a los tres días.
  - **[`ERRORES.md`](../ERRORES.md)** — *"¿esto que voy a escribir es una instancia de algún patrón?"* Son las trampas en las que este proyecto YA cayó, varias dos veces.
  - **[`VERSIONES.md`](../VERSIONES.md)** — *"¿ya se creó o se tocó algo parecido, cuándo, y por qué se hizo así?"* Sirve incluso al **crear** algo nuevo, no solo al modificar: si hace dos versiones se construyó un mecanismo similar, lo que se aprendió allí aplica aquí — y si lo que vas a crear se parece mucho a algo que ya existe, quizá no toque crear sino extender.
  - **Y de un archivo al otro:** cuando `VERSIONES.md` diga que una versión cerró ciertos patrones, ve a leerlos. Es la vía más rápida para no repetir los errores que rodearon a un cambio parecido.
- ¿Duplica algo que ya existe? (Si sí, extiende lo existente en vez de crear un paralelo.)
- ¿Tiene **invocador**? ¿Qué lo dispara, y está cableado?
- **¿Ese invocador corre SOLO, o depende de que alguien se acuerde?** Un invocador **manual** (un comando que el usuario escribe, una pregunta que tiene que hacer) **NO cuenta como cableado**: en una sesión fría nadie lo escribe, y la regla no existe. Si la regla es cara y no cabe en el arranque, entonces **su vencimiento tiene que vigilarlo algo barato que sí corra solo** — no basta con recomendar una frecuencia, porque una recomendación sin campo que la registre no tiene memoria.
- **¿En qué MOMENTO exacto hace falta, y el protocolo que corre en ese momento la invoca?** Esta es distinta de "¿tiene invocador?" y es la que más se salta. Una regla puede estar perfectamente escrita, tener invocador, y aun así no dispararse nunca — porque vive en el protocolo de una acción y hace falta durante **otra**. Escribe la regla donde se NECESITA, no donde encaja temáticamente; si su sitio natural es otro archivo, deja en el punto de uso un puntero explícito que ordene ir a leerla.
- ¿Su condición es **verificable por máquina**, o exige juicio? (El README exige aritmética verificable, no juicio.)
- ¿Tiene **interruptor de apagado**? Una alerta que no se puede callar se acaba ignorando.
- ¿El formato va **literal**, con especimen copiable?
- ¿Va al **repo**, no a la memoria del agente?

### Rama 2B — MODIFICAR algo que ya existe
Todo lo de 2A, **más el arrastre** — el peligro que solo tiene modificar:
- ¿Qué **otros archivos apuntaban a esto**?
- ¿Qué queda **obsoleto o contradictorio** en otro sitio después de este cambio?
- ¿Algún puntero quedó **huérfano**?
- **¿Qué dice [`VERSIONES.md`](../VERSIONES.md) de la última vez que se tocó esto, y por qué?** Búscalo antes de cambiarlo. Sirve para dos cosas concretas: no **revertir sin querer** un arreglo reciente (si algo se cambió hace tres días para cerrar un patrón, volver atrás lo reabre), y no **re-arreglar** lo que ya está arreglado. Si lo que vas a tocar aparece en la versión más reciente, para y entiende por qué se puso así antes de moverlo.
- **¿Quién más LEÍA el campo o la lista que acabo de vaciar?** Un campo que deja de usarse en un sitio puede seguir siendo la única fuente de otro, y vaciarlo lo deja ciego en silencio. (Caso real, patrón P9: quitar los cursos base de `cursos_requeridos` arregló la viabilidad y dejó al PASO 5.5 sin forma de ver esos cursos, porque el selector lee `cursos_requeridos` y no la cerradura.)

*(Ya pasó: al quitar un punto de `PENDIENTE.md`, `CLAUDE.md` quedó diciendo "dos cosas". Se cazó por suerte, no por procedimiento.)*

**Caso ambiguo** (agregar algo nuevo dentro de un archivo que ya existe): corre **las dos ramas**.

### Cierre obligatorio de toda la Capa 2 (2A y 2B)

Un cambio de Capa 2 **no está terminado** hasta hacer estas dos cosas, en el mismo turno:

1. **Corre la Rama 2C** (simulacro contra el estado real — abajo). Sin esto no puedes decir que funciona, solo que está escrito.
2. **Escribe la entrada en [`VERSIONES.md`](../VERSIONES.md)**: qué se añadió, cambió o eliminó. Si el cambio cerró un error, documéntalo en `ERRORES.md` y desde `VERSIONES.md` solo enlázalo — **nunca describas lo mismo en los dos**.

Este cierre es el **invocador de escritura** de `VERSIONES.md`. Sin él, ese archivo se queda viejo en tres cambios y se convierte en algo peor que no tenerlo: un registro que parece autoritativo y miente.

### Rama 2C — SIMULACRO OBLIGATORIO (después de escribir, antes de decir "listo")

> **⛔ PUERTA DE SALIDA — la regla que hace que esto no se pueda saltar.**
>
> **NO PUEDES ANUNCIAR UN CAMBIO DE CAPA 2 COMO HECHO SIN HABER EMITIDO LA TABLA DEL SIMULACRO EN ESE MISMO MENSAJE.** Ni "listo", ni "quedó escrito", ni "lo añadí". Si en tu respuesta no aparece la tabla, el cambio **no está terminado** y decir lo contrario es reportar mal.
>
> **Por qué está redactada así** (fallo real, cuatro veces el 2026-07-18, con contexto completo): la Rama 2C existía, estaba bien escrita, y se saltó una y otra vez. El motivo no fue el olvido: **este control no dejaba rastro.** El PRE-FLIGHT se auto-evidencia —si no corre, faltan las líneas de alerta y se nota—; el simulacro no producía nada, así que el archivo escrito **se veía idéntico** con simulacro o sin él. Un control cuya ejecución es invisible es indistinguible de uno que se omitió, y por eso se omite: no cuesta nada y nadie puede comprobarlo. Además se disparaba *al terminar de escribir*, que es cuando la atención está gastada y el trabajo "se siente hecho".
>
> **Atarlo al REPORTE lo arregla**, porque el reporte es lo único que el agente no puede omitir: siempre le cuenta al usuario qué hizo. La tabla es el rastro. **Si el usuario no ve tabla, sabe que se saltó** — y eso es lo que convierte esto en un control de verdad y no en una buena intención.

**Ninguna regla está terminada hasta ejecutarla contra el estado real del proyecto.** Revisarla no basta: los dos errores del 2026-07-18 sobrevivieron a la lectura y murieron al primer contacto con los datos.

Qué hacer, en concreto — esto NO es una metáfora, es un procedimiento:

1. **Toma el estado real de HOY**, leyendo los archivos: `objetivo.json`, los `progreso.json` y `temario.json` de los cursos, `historial.json`. No un ejemplo inventado, no "supongamos que" — los datos que hay ahora mismo en el repo.
2. **Recorre tu regla nueva paso a paso contra esos datos** y escribe el resultado caso por caso: para cada condición, si se cumple o no **y con qué números**. Si la regla es una alerta, di cuáles disparan y cuáles no. Si es un filtro, di a qué excluye por nombre.
3. **Enséñale al usuario esa tabla**, no un resumen. "Lo probé y funciona" no es un simulacro; es la afirmación que el simulacro tenía que respaldar.
4. **Mira específicamente tres cosas**, que son las que fallaron:
   - **Ruido:** ¿cuántas líneas produce? Si una alerta se cumple para muchos elementos a la vez, agrégala en una — doce líneas entierran a la que importaba.
   - **Silencios falsos:** ¿a quién deja fuera tu condición, y es correcto que quede fuera? Recórrelos **por nombre**. Un filtro que silencia algo del objetivo prioritario es un error grave disfrazado de limpieza.
   - **PRUEBA DE SESIÓN FRÍA — ¿hay que INFERIR algo para ejecutarla?** Relee lo que escribiste como si no supieras nada de esta conversación y marca cada punto donde un agente tendría que **decidir qué quisiste decir**: un formato descrito en prosa y no con especimen, un umbral sin número, un "cuando corresponda", un puntero a "el protocolo correspondiente" sin nombrar el archivo, un término que en este proyecto significa dos cosas. **Cada inferencia es un error futuro**, porque distintas sesiones infieren distinto y ninguna sabe que está infiriendo. Si encuentras una, no la expliques mejor: **escríbela literal** (Capa 1: *principios para decidir, literal para escribir*).

5. **Deja constancia de las tres**, aunque el resultado sea limpio. "Sin hallazgos" es un resultado; no decir nada no lo es.

**Especimen literal de la tabla** (este es el rastro que exige la puerta de salida — cópialo tal cual, no lo describas ni lo reinventes):

```markdown
**Simulacro (Rama 2C) — <qué regla se cambió>**

| Comprobación | Resultado |
|---|---|
| Ejecución contra el estado real | <qué dispara y qué no, CON NÚMEROS y nombres> |
| Ruido (nº de líneas que produce) | <N líneas — agregada / sin agregar / no aplica> |
| Silencios falsos (a quién deja fuera) | <nombres concretos, y por qué es correcto> — o "ninguno" |
| Prueba de sesión fría (inferencias) | <cada punto donde habría que adivinar> — o "ninguna" |
```

Si una fila no aplica al cambio (p. ej. una regla que no produce salida no tiene "ruido"), escribe **"no aplica"** y por qué. **Lo que no vale es omitir la fila**: una tabla incompleta se lee igual que una completa y vuelve a hacer invisible lo que no se hizo, que es el fallo exacto que esta puerta existe para cerrar.

**Ejemplo real de por qué existe (2026-07-18):** se escribió un filtro `hoy >= fecha_inicio` para una alerta. Leído, era razonable. Ejecutado contra los datos reales, silenciaba los 5 cursos del objetivo PRIORITARIO — porque `fecha_inicio` significa "cuándo empezó el estudio" en un objetivo y "cuándo empiezan las clases en la universidad" en otro. **Ningún repaso lo habría cazado; el simulacro sí, en un minuto.**

> **Nota para quien lea esto sin contexto:** en el `README.md`, la regla 6 del PASO 5.5 se llama **PRE-FLIGHT** y hace exactamente esto pero en cada arranque de sesión, con las alertas ya escritas. Esta Rama 2C es lo mismo aplicado a una regla **que acabas de escribir y que todavía no existe en ningún sitio**. Si lo que cambiaste fue una alerta, el simulacro es literalmente correr el PRE-FLIGHT.

---

## Capa 3 — EJECUTAR un procedimiento ya definido

**Criterio (principio, no lista):** toda acción para la que **ya exista un procedimiento definido** en este proyecto. Un protocolo nuevo entra solo, sin tocar esta capa.

**Relee ENTERO el protocolo de esa acción antes de ejecutarla**, aunque creas que te lo sabes — que es justo cuando falla.

*Inventario actual (ilustración, NO la definición): arranque, selector de clase, inicio y cierre de clase, handoff, coordinación con la agenda, configuración, utilidades, entrevista, expansión.*

### Frontera con la Capa 2
El corte es **datos vs reglas**:
- Ejecutar un procedimiento que escribe **datos** (`progreso.json`, `historial.json`, `glosario.json`) es Capa 3 pura: el protocolo **es** el checklist.
- Cambiar **qué datos existen o qué reglas los rigen** (un campo nuevo, un protocolo, el README) es Capa 2.

---

## Restricciones vigentes

Restricciones que **no viven dentro de ningún protocolo** y que por eso se saltan solas. Cada una lleva **condición de retiro** y **verificador** (quién lo sabe y cómo nos enteramos), porque el fallo real no es que pase la fecha: es que la condición se cumpla y nadie avise.

**Barrido:** las que dependen de Ángela se preguntan en el **sync dominical** (ver `coordinacion-agenda.md`); las demás las levanta el bloque de alertas de `estado`, o el usuario. **Si una restricción se retira, bórrala de aquí** — una lista sin caducidad se pudre igual que una regla por casos.

| Restricción | Desde | Condición de retiro | Verificador |
|---|---|---|---|
| MacroDroid no está operativo; las alarmas las pone Ángela directo | 2026-07-17 | Ángela confirma que lo arregló | Ángela, vía sync (o el usuario) |
| Las fechas de parcial (~28-sep) y final (~23-nov) son PROVISIONALES: el inicio 10-ago puede correrse al 17-ago y todo corre ~1 semana | 2026-07-17 | Ángela verifica contra el calendario oficial en la semana 1 de clases | Ángela, vía sync |
| La fecha del examen UNI (~15-feb-2027) es PROVISIONAL | 2026-07-17 | La UNI publica el proceso de admisión 2027-1 | El usuario o Ángela |
| Los 5 temarios del IV ciclo son INFERIDOS (sin sílabo publicado) | 2026-07-16 | `silabo_validado.validado == true` en cada `temario.json` | Verificable solo (alerta 14 de `estado`) |
| El ritmo real (h/sesión, min/repaso) NO está medido: las viabilidades son supuestos | 2026-07-16 | `historial.json` con ≥5 sesiones (checkpoint del 26-jul-2026) | Verificable solo (alertas 10 y 11 de `estado`) |
| La ventana de 5 h de rate limit se comparte con el proyecto de Ángela; ninguno de los dos puede leer el consumo | 2026-07-17 | Estructural mientras compartan cuenta — no se retira | — |
| El objetivo de la UNI (obj-1) está en `latente`: sus 8 cursos no compiten por horas. Se aceptó que el examen del 15-feb **puede no alcanzar** a cambio de salvar el parcial | 2026-07-18 | Su `condicion_activacion`: obj-2 completado, **o** `hoy >= fecha_limite` de obj-2 | Verificable solo (PASO 2a del arranque + alerta 7 de `estado`) |

---

## Invocación

Lo dispara el **PASO 0** del algoritmo de arranque del `README.md`, y `CLAUDE.md` lo señala como lectura obligatoria de cada sesión. Si este archivo se mueve o renombra, actualizar ambos.
