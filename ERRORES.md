# Errores del proyecto — patrones, no anécdotas

> **Qué es esto.** El catálogo de los errores que este proyecto ha cometido, organizados por **patrón** para poder reconocerlos ANTES de repetirlos. No es un diario de incidentes: es una lista de trampas con nombre.
>
> **Por qué en un solo archivo, y por qué patrones.**
>
> **Una lista de incidentes no se usa.** Cincuenta entradas fechadas son ilegibles en el momento de decidir; nueve patrones con nombre sí se reconocen. Por eso lo que manda aquí es el **mecanismo de fallo**, y los incidentes son solo sus ejemplos — podables cuando sobran.
>
> **Corrección (2026-07-18, señalada por el usuario).** La primera versión de esta nota justificaba no escribir historial diciendo *"el changelog ya existe: es git"*. **Es un argumento inválido y es literalmente el patrón P1 de este archivo:** git registra los cambios, pero **nada invoca a git**. Nunca se ha revisado el historial para buscar si un error ya había ocurrido, y no hay ningún disparador que lo pida. Una función cubierta por un mecanismo sin invocador **no está cubierta**. Que el archivo que documenta ese patrón cayera en él el día que se escribió dice lo bien camuflado que va.
>
> Lo que sí es cierto y sostiene la decisión: el detalle **por commit** vive en git y no vale la pena duplicarlo a mano — pero no porque git lo "cubra", sino porque ese grano no es el que se usa al decidir. El grano que se usa es el de **patrón** y el de **versión de arquitectura**, y los dos viven aquí, dentro del archivo que sí tiene invocador.

---

## INVOCADOR (sin esto, este archivo es decoración)

Se usa en **tres momentos**, los tres ya cableados:

1. **Capa 2 de `_protocolos/capas.md`** (crear o modificar reglas/estructura): antes de escribir, pasa el cambio por los patrones de abajo. La pregunta literal es: **"¿esto que voy a escribir es una instancia de alguno de estos patrones?"**
2. **Cuando el usuario reporta que algo volvió a fallar**: busca aquí primero. Si el fallo encaja en un patrón conocido, dilo — que un error sea recurrente cambia el arreglo (no basta parchear el caso; hay que preguntarse por qué el patrón sigue vivo).

3. **En el momento en que se detecta un error** — no al cerrar la sesión, no "cuando haya tiempo". Ver el procedimiento justo abajo. Lo invoca la **Capa 1** de `capas.md`, que corre en cada mensaje.

---

## Cómo se documenta un error nuevo (procedimiento)

> **Se dispara SIEMPRE que aparece un error del proyecto**, sin importar quién lo encuentre (el usuario o el agente), en qué tipo de sesión (fría o con contexto), ni si se arregla en el momento o no. **Un error detectado y no escrito se pierde**, y este proyecto ya ha repetido tres patrones distintos por eso.

**Regla de oro: se escribe ANTES de arreglarlo, no después.** Mientras se arregla, la atención se va al arreglo y el error se da por sabido; al terminar, lo que se recuerda es la solución, no la trampa. Y si la sesión se corta a mitad del arreglo, se pierden los dos.

1. **¿Encaja en un patrón que ya existe?** Léelos. En este proyecto la respuesta ha sido "sí" la mayoría de las veces.
   - **Sí** → añádelo como **caso** de ese patrón, con su fecha. **No crees un patrón nuevo.** Una lista con veinte patrones de un caso cada uno es una lista de incidentes disfrazada, y no se usa.
   - **No** → crea un patrón nuevo con la plantilla de abajo. El listón para crear uno es alto: tiene que ser un **mecanismo de fallo**, no un suceso.
2. **Escríbelo en términos de mecanismo, no de anécdota.** "Se me olvidó mirar X" no sirve a nadie. "X vivía en el protocolo de otra acción y nada apuntaba hacia allá" sí, porque se reconoce la próxima vez.
3. **Si el error ya se arregló**, di **dónde vive el arreglo** (archivo y sección) para que se pueda verificar que sigue ahí. Si **no** se arregló, márcalo **ABIERTO** y escribe la consecuencia de dejarlo suelto.
4. **Si cerraste un patrón o cambiaste la arquitectura**, escribe la entrada en [`VERSIONES.md`](VERSIONES.md) — no aquí. Desde allí se enlaza al patrón; **nunca describas lo mismo en los dos archivos**.
5. **Que lo escriba quien lo detecta.** Si el usuario caza el error, el agente lo documenta en ese turno — no espera a que se lo pidan.

**SI EL ERROR APARECE EN MITAD DE UNA CLASE DE ESTUDIO, NO INTERRUMPAS LA CLASE.** Parar una clase para depurar el proyecto es la **deriva por curiosidad** que el README manda contener, y encima le come horas al objetivo prioritario.

Escríbelo **en la sección "Bandeja de entrada" del final de este archivo**, con esta marca literal al principio de la línea:

```markdown
- `[SIN ANALIZAR]` *YYYY-MM-DD* — <qué pasó y qué lo delató, dos líneas máximo>
```

Y sigue con la clase. **La marca `[SIN ANALIZAR]` no es decorativa: es lo que hace que la nota no se pudra.** La **alerta 17** (BARATA, corre en cada arranque) cuenta esas marcas y las saca a la superficie hasta que alguien las procese. Sin ella esto sería una nota que tú olvidas, yo olvido, y nadie vuelve a leer — exactamente el patrón P1 dentro del archivo que lo documenta.

**Procesarlas** = en una sesión de arquitectura, convertir cada una en caso de un patrón (o en patrón nuevo), y **borrar la línea de la bandeja**. La bandeja vacía es el estado normal.

**PODA:** los patrones son permanentes; los **casos no**. Cuando un patrón acumule más de ~5 casos, deja los **dos más ilustrativos** (normalmente el primero y el más reciente) y resume el resto en una línea del tipo *"+3 casos más entre YYYY-MM y YYYY-MM"*. Una lista de casos que crece sin tope acaba siendo el registro de incidentes que este archivo existe para NO ser — y deja de leerse, que es la única forma en que puede fallar.

**Plantilla literal para un patrón nuevo** (cópiala tal cual):

```markdown
## PN — <nombre corto del MECANISMO de fallo>

**Síntoma:** <qué se ve desde fuera cuando está pasando>

**Por qué pasa:** <la causa estructural, no la anécdota>

**Casos reales:**
- *YYYY-MM-DD* — <qué pasó, con cifras si las hay, y quién lo cazó>

**Cómo se caza:** <la pregunta concreta que lo detecta antes de cometerlo>

**Dónde vive el arreglo:** <archivo y sección> — o **ABIERTO** + consecuencia de dejarlo así
```

**Y para un caso dentro de un patrón que ya existe**, basta una línea en su lista de casos:

```markdown
- *YYYY-MM-DD* — <qué pasó y qué lo delató>
```

---

## Versiones

**Viven en [`VERSIONES.md`](VERSIONES.md), no aquí.** Ese archivo dice *qué tiene el proyecto en cada versión* (añadido / cambiado / eliminado) y enlaza a los patrones de abajo que cada versión cerró.

La tabla de versiones estuvo aquí unas horas el 2026-07-18 y se sacó: un versionado hecho solo de "patrones cerrados" **no ve las adiciones** —el protocolo de coordinación con la agenda, `historial.json`, la escalera de repaso no fueron arreglos de errores— y describir las versiones en dos sitios garantiza que uno se quede viejo. **Aquí: patrones de fallo. Allí: qué hay y desde cuándo.** El enlace va en un solo sentido, de versiones a patrones.

---

## P1 — Regla sin invocador AUTOMÁTICO

**Síntoma:** la regla está escrita, es correcta, y no dispara nunca. Cuando falla y se investiga, aparece perfectamente redactada en su sitio.

**Por qué pasa:** se le dio como invocador un comando que el usuario tiene que escribir, o una pregunta que tiene que hacer. En la sesión con contexto rico eso no se nota, porque el agente conecta solo. En una sesión fría nadie escribe el comando y la regla no existe.

**Casos reales:**
- *2026-07-18* — Las 14 alertas del comando `estado` dependían todas de que el usuario escribiera `estado`. La **alerta 9** detectaba exactamente el problema que estaba ocurriendo (objetivo prioritario cediendo ante uno secundario) y no disparó: el agente propuso una clase de un objetivo secundario **teniendo leído en su contexto el déficit de 52 h del prioritario**. El usuario tuvo que sacárselo a la fuerza en dos mensajes.
- *2026-07-18* — `"Sugiere correr estado cada 2-3 semanas"` era una recomendación **sin memoria**: ningún campo registraba la última ejecución, así que una sesión fría no podía saber si las cifras que usaba eran de ayer o de hace un mes.

- *2026-07-18, **tercera instancia, cometida al escribir este mismo archivo***. Se justificó no llevar historial de cambios diciendo *"el changelog ya existe: es git"*. El usuario contestó: **"¿y cuándo se revisa git? ¿alguna vez se ha hecho?"** Nunca. Ni una vez. Git registra, pero **nada invoca a git**: no hay disparador que mande consultarlo cuando aparece un error. Dar una función por cubierta con un mecanismo sin invocador es este patrón exacto — y coló dentro del archivo que lo documenta.

**Cómo se caza:** checklist de Capa 2A — *"¿el invocador corre solo o depende de que alguien se acuerde?"* Un invocador manual **no cuenta como cableado**. Y su forma más camuflada: **"eso ya lo cubre X"**, donde X es una herramienta que nadie consulta. Pregúntate siempre *quién* mira X y *qué* le manda mirarlo.

**Dónde vive el arreglo:** PRE-FLIGHT (regla 6 del PASO 5.5) + tabla BARATAS/CARAS y alerta 15 en `_protocolos/utilidades.md` + `objetivo.json → ultima_auditoria`.

---

## P2 — La regla vive en el protocolo de OTRA acción

**Síntoma:** el agente lee todo lo que le toca, clasifica bien la acción, relee el protocolo correcto… y falla igual.

**Por qué pasa:** al escribirla, se la colocó donde encajaba **temáticamente** ("esto es re-planificación", "esto es una alerta") en vez de **donde hace falta operativamente**. En una sesión con contexto no importa; en una fría, la ubicación *es* la funcionalidad.

**Casos reales:**
- *2026-07-18* — La palanca de "aflojar la cadencia del objetivo menos prioritario" estaba en la sección de Re-planificación del README, a ~500 líneas del PASO 5.5, que es donde se necesitaba. El agente releyó el PASO 5.5 entero y no tenía forma de saber que existía.

**Cómo se caza:** checklist de Capa 2A — *"¿en qué momento exacto hace falta, y el protocolo que corre en ese momento la invoca?"* Si su sitio natural es otro archivo, **deja un puntero explícito en el punto de uso**.

**Ojo:** este patrón es la razón de que `capas.md` sea necesario pero **no suficiente**. Leer más no lo arregla; cablear sí.

---

## P3 — La decisión se queda en el chat

**Síntoma:** se acuerda algo, la sesión termina, y a la siguiente no existe.

**Por qué pasa:** en la conversación la decisión *se siente* tomada. Pero el estado del proyecto vive en archivos, y el usuario puede cambiar de agente.

**Casos reales (tres el mismo día):**
- *2026-07-18* — Se acordó "relajamos la cadencia, no la matamos" y no se escribió el número ni se tocó el `temario.json` hasta que el usuario siguió preguntando.
- *2026-07-18* — La regla del simulacro obligatorio se **propuso** y el agente dio a entender que estaba puesta. No lo estaba. El usuario preguntó *"creo que ya lo has agregado, ¿no?"* y la respuesta honesta era **no**.
- *2026-07-18* — Los números del cálculo de horas liberadas (~21 h) vivían solo en el chat hasta que se guardaron en `_cadencia_temporal`.
- *2026-07-22 (latente, cazado antes de morder)* — El formato de RECONCILIACIÓN de `coordinacion-agenda.md` (cerrado el 17-jul) exige reportar `Temario agendado/real`, pero el `agendado` es un número **negociado con Ángela, no derivable de ningún dato del repo**, y no se le dio hogar en ningún archivo del lado Richard. Es decir: la RECONCILIACIÓN del sync N+1 **no podía producirse en una sesión fría** — su input vivía solo en el chat y en el calendario de Ángela. No disparó antes porque este (2026-07-22) fue el **primer** sync y aún no había un segundo que reconciliar. → Arreglado creando `sync-estado.json` (ver [`VERSIONES.md`](VERSIONES.md) v3.3).

**Cómo se caza:** Capa 1 — *"el estado vive en ARCHIVOS del repo"*. Regla práctica: **si al terminar la sesión la decisión no está en un archivo, no se tomó.** Y nunca digas "ya está agregado" sin haberlo escrito en ese mismo turno. Variante estructural (caso del 22-jul): cuando un formato pide reportar un dato, pregúntate **de dónde lo lee una sesión fría** — si la respuesta es "del chat" o "de la memoria del otro agente", falta un archivo.

---

## P4 — Un campo respondiendo DOS preguntas

**Síntoma:** una condición que parece correcta hace algo absurdo en un caso concreto.

**Por qué pasa:** el campo significa una cosa en un sitio y otra en otro, y quien escribió la condición miró solo el primero.

**Casos reales:**
- *2026-07-16* — El "dueño" de un curso respondía a la vez *"¿quién lo creó?"* y *"¿quién lo necesita y para cuándo?"*. Trigonometría heredaba la fecha lejana de la UNI cuando el semestre la necesitaba ya. → Se sustituyó por **urgencia efectiva**.
- *2026-07-18* — `fecha_inicio` significa *"cuándo empezó el estudio"* en el objetivo de la UNI y *"cuándo empiezan las clases en la universidad"* en el del semestre. Se usó como filtro de una alerta y **silenciaba los 5 cursos del objetivo prioritario**. Lo cazó el usuario, el mismo día en que se escribió.

**Cómo se caza:** antes de usar un campo como condición, **verifica qué significa en CADA objetivo/curso donde aparece**, no en el primero que mires.

---

## P5 — Contar solo una parte del coste

**Síntoma:** el plan dice "cabe" y no cabe. La aritmética está bien; falta una partida entera.

**Casos reales:**
- *2026-07-16* — Las validaciones contaban `horas_aprender` e ignoraban `horas_mantener`. Con 115 temas, la escalera dispara ~428 repasos (~70-85 h) que el plan no veía, contra un margen reportado de 18 h.

**Cómo se caza:** al validar un deadline, pregunta **qué trabajo real ocurre que no esté en la fórmula**. Los métodos estaban documentados y aplicados; simplemente no estaban presupuestados.

---

## P6 — Contador de esfuerzo en vez de estado verificable

**Síntoma:** el sistema cree que algo está aprendido porque se le dedicaron sesiones.

**Casos reales:**
- *2026-07-16* — `pagado[tema] = sesiones` decidía si un tema estaba listo. Un usuario que estudia tres veces y sigue fallando quedaba marcado "pagado, siguiente". Lo señaló el usuario.

**Cómo se caza:** *"¿esto mide lo que ocurrió, o lo que se gastó?"* El esfuerzo no es verificable; el estado sí. `nivel_alcanzado` solo sube contra evidencia.

---

## P7 — Sobre-especificar: jalar cursos enteros donde bastan temas

**Síntoma:** el plan se ve imposible cuando no lo es.

**Casos reales:**
- *2026-07-16 y de nuevo 2026-07-17* — El semestre listaba los cursos base **enteros** en `cursos_requeridos`, cuando sus cursos solo dependen de 27 temas concretos por prerrequisito. Inflaba el parcial en ~82 h. **Se cometió dos veces.**

**Cómo se caza:** alerta 12 de `utilidades.md`. Prerrequisitos **siempre a nivel de tema**, nunca de curso (regla 4 del checklist del temario).

---

## P8 — Ruido que apaga la red de seguridad

**Síntoma:** las alertas siguen ahí y nadie las lee.

**Casos reales:**
- *2026-07-18* — Al cablear las alertas al arranque, la alerta 2 producía **12 líneas por sesión** (un curso sin tocar = una línea). Habría enterrado a la alerta que sí importaba. Se detectó **ejecutando** el PRE-FLIGHT, no revisándolo.

**Cómo se caza:** Rama 2C (simulacro) — cuenta cuántas líneas produce tu regla contra el estado real. Si una alerta puede cumplirse para muchos elementos, **agrégala en una sola línea**. El ruido no es un problema estético: es cómo una red de seguridad se apaga sola sin que nadie la apague.

---

## P9 — Arreglar una cosa reintroduce otra *(CERRADO 2026-07-18)*

**Síntoma:** un arreglo correcto destapa un bug en otra capa que dependía silenciosamente del estado anterior.

**Casos reales:**
- *2026-07-17 → detectado 2026-07-18* — Para matar P7 se quitaron los cursos base de `cursos_requeridos` del semestre, dejando que entren por cerradura. Correcto para la **viabilidad**. Pero el **PASO 5.5 recorre `cursos_requeridos`, no la cerradura** — así que los temas base volvieron a heredar la urgencia de la UNI (feb 2027) en vez de la del parcial (sep 2026). **Es P4 resucitado por la puerta de atrás.**
  **Estado: CERRADO el 2026-07-18.** El PASO 5.5 ahora calcula su universo de candidatos con `cerradura()` —la MISMA función de la viabilidad, no una copia— y la **urgencia efectiva pasó a ser por TEMA, no por curso** (dar al curso entero la urgencia del semestre habría resucitado P7: de los 17 temas de álgebra el semestre necesita 8). La rama F además comprueba ahora los `prerrequisitos_externos` **al elegir**, no después.
  **Tercera consecuencia que apareció al arreglarlo:** la rama F solo miraba prerrequisitos INTERNOS. Podía proponer un tema cuyas bases externas no existían, y el "Protocolo al INICIAR" se limitaba a dudar en voz alta ("probablemente no es el que toca") sin ruta de vuelta.
  **Efecto sobre `latente`:** cerrado P9, poner el objetivo de la UNI en `latente` **ya es seguro** — los temas base siguen siendo alcanzables por la cerradura del semestre. Antes los habría hecho desaparecer.

- *2026-07-19* — Al añadir la Prioridad 1 (migrar los protocolos a código) a `PENDIENTE.md` el 18-jul, `CLAUDE.md` quedó diciendo que ese archivo quedaba "por **una sola cosa**" (el porteo al template). El puntero mintió un día entero; lo cazó la revisión completa previa a crear `PLAN-MOTOR.md`, no ningún mecanismo del proyecto. Es la misma mecánica del caso ya anotado en la Rama 2B de `capas.md` ("al quitar un punto de `PENDIENTE.md`, `CLAUDE.md` quedó diciendo 'dos cosas'"), ahora en sentido inverso (al añadir). Arreglado el mismo 19-jul corrigiendo `CLAUDE.md`. Ojo: el verificador de punteros del motor (paso 1.2 de `PLAN-MOTOR.md`) cazará enlaces a archivos inexistentes, pero NO afirmaciones semánticas como esta ("queda una sola cosa") — esas siguen dependiendo del barrido de arrastre de la Rama 2B.
- *2026-07-18, **segunda instancia el mismo día, dentro del arreglo de la primera*** — Al arreglar lo de arriba se cableó el selector a `cerradura()` a secas. Pero `cerradura()` **corta en lo ya sabido** (es la respuesta a *"¿qué me falta aprender?"*), y el repaso pregunta lo contrario: *"¿qué tengo que mantener vivo?"*, donde lo ya sabido **es** el objeto. Resultado: los **7 temas de aritmética ya estudiados de los que depende el semestre** se caían del universo de repaso y se habrían desvanecido en silencio — 12 sesiones de trabajo perdidas, sin que ninguna alerta se quejara. Se cazó con el simulacro de la Rama 2C, no leyendo. → Arreglado partiendo en dos conjuntos: `aprender()` (corta) y `mantener()` (no corta).

**Cómo se caza:** Rama 2B — *"¿qué queda obsoleto o contradictorio en otro sitio después de este cambio?"*, y en particular: **¿quién más leía el campo que acabo de vaciar?** Un campo que deja de usarse en un sitio puede seguir siendo la única fuente de otro.

**Lección propia de este patrón:** es el más traicionero de la lista porque **aparece DENTRO del arreglo de otro error**, cuando la guardia está baja y uno se siente productivo. Ocurrió dos veces en un mismo día. **Regla práctica: un arreglo estructural no está terminado hasta simular también lo que el arreglo NO tocaba** — la pregunta no es solo "¿funciona lo nuevo?" sino "¿qué dependía en silencio de cómo era antes?".

---

## Bandeja de entrada — casos SIN ANALIZAR

> Aquí caen los errores detectados **en mitad de una clase de estudio**, escritos en bruto para no interrumpirla (ver el procedimiento de arriba). No llevan patrón asignado todavía: eso se hace en una sesión de arquitectura.
>
> **La marca `[SIN ANALIZAR]` es lo que los mantiene vivos:** la alerta 17 de `_protocolos/utilidades.md` las cuenta en cada arranque y las saca a la superficie hasta que se procesen. **Procesar** = convertirlo en caso de un patrón (o en patrón nuevo) y **borrar la línea de aquí**.
>
> **El estado normal de esta sección es VACÍA.** Si acumula entradas, es que se están detectando errores más rápido de lo que se analizan.

*(vacía)*
