# Protocolo de utilidades

> Comandos rápidos de uso frecuente para consultar el estado del proyecto o hacer mantenimiento (backup). No cambian la configuración ni requieren flujos largos.

---

## Referencia rápida de comandos

| Comando completo | Alias corto | Qué hace |
|---|---|---|
| `mostrar estado` | `estado` | Muestra un resumen del progreso actual: objetivo, cursos, tema en curso, dificultades pendientes prioritarias |
| `generar backup` | `backup` | Genera un ZIP con el proyecto completo en `_backups/YYYY-MM-DD-HHMM.zip` |

**Regla:** el agente acepta indistintamente forma completa y alias. Si el usuario dice algo parecido pero no idéntico, pide la forma exacta. Ningún comando usa `/` (ver convención en el `README.md` raíz).

---

## Comando `estado`

Lee y agrega información de varios archivos para dar una foto rápida del progreso. Se usa cuando el usuario abre el proyecto y quiere saber por dónde va sin arrancar una sesión de estudio.

### Pasos del agente

1. Lee `objetivo.json` → `presupuesto_horas` (línea de tiempo de periodos) y **todos** los `objetivos` con su `estado`, `prioridad`, `hitos` y `cursos_requeridos` (con sus `nivel_requerido` por curso y los `overrides` por tema). Agrupa la salida por objetivo, ordenada por prioridad, y marca los `latente` y `completado` como tales.
1b. Detecta los **cursos compartidos**: los que aparecen en el `cursos_requeridos` de más de un objetivo activo. Merecen mención propia: son los que rinden doble.
2. Lee `configuracion.json` → estado de las reglas.
2b. Lee `historial.json` y calcula el **ritmo real**:
   - `horas_por_sesion` = promedio de `duracion_min`/60. Con <5 sesiones, no lo uses: di "aún sin medir" y usa 1.5 h declarándolo como suposición.
   - `minutos_por_repaso` = **también se mide, no se inventa.** Toma las sesiones con `tipo_clase == "repaso"` y calcula `promedio(duracion_min / nº de temas repasados en esa sesión)`. Con <3 sesiones de repaso, di "aún sin medir" y usa **12 min** declarándolo como suposición. Este número alimenta el `horas_mantener` de las viabilidades de abajo, así que si es supuesto, dilo ahí también.
   - `horas_por_semana_real` = suma de duraciones de las últimas 4 semanas / 4. Con <3 semanas de datos, di "aún sin medir".
   - `horas_por_semana_real` **por objetivo** (filtrando por `objetivo_id`): revela a qué se está yendo el tiempo de verdad, que puede no ser lo que dice la prioridad.
3. Para cada curso requerido por algún objetivo activo (si lo requieren varios, muéstralo una vez y marca cuáles):
   - Lee `temario.json` del curso → campo `modo_estudio`.
   - Lee `progreso.json` del curso.
   - Cuenta temas por estado de flujo (`no_iniciado`, `en_progreso`, `completado`).
   - **Cuenta la PROFUNDIDAD real vs la requerida**, que es lo que de verdad dice si el curso está listo: por cada objetivo activo que lo pide, cuántos de sus temas ya tienen `nivel_alcanzado >= nivel_requerido` (curso + `overrides`) y cuántos no. "Listo para el semestre" y "listo para la UNI" son cuentas distintas sobre el mismo curso: muéstralas por separado si lo piden varios objetivos.
   - Suma `sesiones_dedicadas`.
   - Calcula la **última sesión del curso** = el `ultima_sesion` más reciente entre sus temas (o "nunca" si todos son `null`).
   - Lee `dificultades.json` del curso y cuenta las que tienen `resuelto: false`.
3b. **Viabilidad y predicción de olvido.** Para cada objetivo activo, en orden de prioridad, y dentro de él cada hito no condicional por fecha, corre la **cola con prioridad** del README ("Los objetivos compiten en una cola con prioridad"): calcula `horas_aprender` (los saltos de nivel que faltan, partiendo del `nivel_alcanzado` real), `horas_mantener` (los repasos que la escalera dispara hasta el hito × `minutos_por_repaso`), descuenta lo ya comprometido por objetivos más prioritarios sin recobrar cursos ya pagados, y decide CABE / NO CABE. En la misma pasada, arma la **predicción de olvido**: por cada tema, si `retención(nivel_repaso proyectado)` es menor que los días desde su último repaso hasta la fecha del hito, ese tema llegará frío. Es un aviso obligatorio, no opcional (ver "La durabilidad" en el README).
4. Corre el **PASO 5.5 del `README.md` raíz** (selector de clase) para saber qué clase tocaría, pero **no la arranques**.
5. Calcula el **bloque de alertas** (ver abajo).
6. Presenta el resumen al usuario con este formato (adaptando cifras reales):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ESTADO DEL PROYECTO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Presupuesto (línea de tiempo, en cola por prioridad):
  [desde] → [hasta]: [N] h/semana — [nota]     ← marca con «AHORA» el periodo vigente
  ...
Ritmo real: [H] h/sesión (medido sobre [N] sesiones | AÚN SIN MEDIR, se asume 1.5)
            [W] h/semana reales vs [D] declaradas → [coherente / desviación del X%]
Configuración: modo una-clase-por-sesión [ACTIVO / INACTIVO]

PRÓXIMO HITO: [nombre] de [objetivo] — [fecha] — faltan [N] semanas

━━ [prioridad P] OBJETIVO [id]: [objetivo_final]   [ACTIVO / LATENTE / COMPLETADO]
    (si LATENTE) Despierta cuando: [condicion_activacion] → [aún no se cumple / ¡YA SE CUMPLE!]
    Hitos:
      · [nombre] — [fecha] — faltan [N] sem — necesita [X] h (aprender [Xa] + mantener [Xm]), quedan [Y] h → [CABE / NO CABE por Z h]
      · [nombre] — [fecha] — ... 
      · [nombre] — condicional, no se planifica
    (viabilidad calculada con [H] h/sesión [medido/SUPUESTO] y [R] min/repaso [medido/SUPUESTO],
     descontando [W] h ya reservadas por objetivos más prioritarios, y sin recobrar los cursos
     ya pagados por ellos)
    Llegarán FRÍOS a [hito]: [tema_id (nivel_repaso proyectado N ≈ retiene D días, y el hito
      cae a E días de su último repaso)], ... → o "ninguno: todos llegan vivos"

    Cursos:
      [Nombre curso 1] — [ruta/cadencia/hito] — [S] sesiones
        └── Profundidad: [X/M] temas ya en el nivel que este objetivo pide ([nivel_requerido])
        └── Última sesión: [fecha o NUNCA]
        └── Tema en curso: [nombre del tema] (nivel_alcanzado: [nivel o —])
        └── Dificultades pendientes: [K]
        └── (si aplica) COMPARTIDO con: [otros objetivos que lo requieren]

━━ [prioridad P] OBJETIVO [id]: ...
    ... (repite por cada objetivo, ordenados por prioridad)

Cursos compartidos ([N]): [curso → objetivos que lo piden], ...
  Estos rinden doble: se estudian una vez y avanzan varios objetivos.

Repasos vencidos ([N]): [tema_id (vencido hace X días)], ...
Próximos repasos: [tema_id (fecha)], ...

⚠ ALERTAS
  [ver reglas del bloque de alertas; si no hay ninguna, escribe "ninguna"]

Próxima clase sugerida: [tipo] — [tema_id o curso]
  Razón: [qué rama del PASO 5.5 ganó y por qué]

Handoff pendiente: [sí (tema X desde fecha) / no]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

7. **Actualiza `objetivo.json → ultima_auditoria`.** Este comando es su **único escritor**. Pon `fecha` = hoy y deja `vence_a_los_dias` como esté (default 21). Es lo que apaga la alerta 15 y lo que permite que el arranque sepa si las cifras de viabilidad que usa están frescas o rancias. **Es un write, no un read: no te saltes este paso por tratarse de un comando "de consulta".** Si te lo saltas, `estado` corre pero el proyecto sigue creyendo que nunca se auditó.

    Especimen literal del campo, en la raíz de `objetivo.json`:

    ```json
    "ultima_auditoria": {
      "fecha": "2026-07-18",
      "vence_a_los_dias": 21,
      "que_cubre": "cola de viabilidad completa + alertas CARAS (8, 9b, 12) del bloque de alertas de _protocolos/utilidades.md",
      "nota": "Lo escribe SOLO el comando 'estado'. La alerta 15 lo vigila: si vence, el arranque lo dice."
    }
    ```

8. **No arrancas ninguna clase después de esto.** Solo muestras el estado. Si el usuario quiere estudiar, invoca la sesión normal por separado.

### Bloque de alertas (obligatorio)

> Esta es la red de seguridad del proyecto y la razón principal por la que `estado` existe. Un curso que nunca se estudia **no se queja**: por eso `07-aptitud-academica` estuvo dos semanas en cero sin que nada lo notara, hasta que el usuario lo preguntó. La arquitectura ya no debería permitirlo, pero ninguna arquitectura se cree a sí misma — este bloque es lo que lo verifica desde fuera.

Emite una alerta por cada caso que se cumpla. **No las omitas por parecer alarmista, y no las inventes por parecer útil.**

### Coste de cada alerta: BARATAS vs CARAS (clasificación operativa)

> **Por qué existe esta clasificación (2026-07-18).** Estas 14 alertas estaban todas detrás de un único invocador: que el usuario escribiera `estado` a mano. En una sesión fría eso no ocurre — el agente ni siquiera sabe que hace 3 semanas que nadie la corre. Resultado real: **la alerta 9 existía, estaba bien escrita, decía exactamente lo que había que decir, y nunca disparó.** El usuario tuvo que sacarla a la fuerza en dos mensajes. La regla no falló; falló su cableado. Ver el item de "invocador automático vs manual" en `capas.md`, Rama 2A.
>
> La solución no es correrlas todas siempre —algunas cuestan una cola de viabilidad entera— sino **partirlas por coste y darle a cada mitad el invocador que aguanta**.

| Coste | Cuáles | Qué necesitan leer | Invocador |
|---|---|---|---|
| **BARATAS** | 1, 2, 3, 4, 5, 6, 7, **9a**, 10, 11, 13, 14, **15**, **16**, **17**, **18** | `objetivo.json`, los `temario.json`/`progreso.json`, `historial.json`. Aritmética directa: comparar fechas, contar temas, promediar duraciones. | **PASO 5.5 del `README.md`, regla 6 — cada sesión, automático.** Y también `estado`. |
| **CARAS** | 8, 9 (versión rigurosa), 12 | Exigen correr la **cola con prioridad completa** (`horas_aprender` + `horas_mantener` sobre el cierre transitivo de cada hito). No cabe en cada arranque. | `estado` a mano, o el **checkpoint fechado** de `objetivo.json → ultima_auditoria`, cuyo vencimiento SÍ lo vigila una alerta barata (la 15). |

**Regla para alertas futuras:** toda alerta nueva declara su coste en esta tabla. **Una alerta sin coste declarado se trata como BARATA** — el default seguro es correrla, porque el fallo caro de este proyecto siempre ha sido el detector que no dispara, nunca el que dispara de más.

1. **Curso de cadencia abandonado** — un curso `cadencia` vencido por más de **3×** su `cadencia_dias`.
   `⚠ [Curso] lleva [N] días sin tocarse (cadencia: cada [C] días).`
2. **Curso nunca tocado** — un curso con todos sus `ultima_sesion` en `null`, el proyecto con más de 2 semanas de vida, **y que sea requerido por al menos un objetivo `activo`.**
   `⚠ [Curso] no se ha tocado NUNCA desde que se creó el plan.`
   **El único filtro legítimo es `estado: "activo"`** (los cursos que solo sirven a objetivos `latente` o `completado` no compiten por horas, así que no van tarde). **NO filtres por `fecha_inicio`.**

   > **Error real, cazado por el usuario el 2026-07-18, el mismo día en que se escribió esta alerta.** Se añadió un filtro `hoy >= fecha_inicio` razonando que "un curso cuyo objetivo no arrancó está esperando turno". Es falso, y peligroso: `fecha_inicio` significa cosas distintas según el objetivo — en obj-1 es cuándo empezó a estudiarse; en obj-2 es cuándo empiezan las **clases en la universidad** (sale de `calendario_oficial.inicio_clases`). El estudio del semestre tiene que ocurrir ANTES de esa fecha: su `_nota_viabilidad` integra las horas **desde hoy** y declara las vacaciones como "la única ventana que decide si el parcial cabe". El filtro silenciaba la alarma de los 5 cursos del objetivo PRIORITARIO — lo contrario de lo que esta alerta existe para hacer. **Lección: antes de usar un campo como condición, verifica qué significa en CADA objetivo, no en el primero que mires.** Un campo con dos significados es el bug que este proyecto ya tuvo con el "dueño" de un curso.

   **AGREGA, no enumeres.** Si se cumple para más de 3 cursos, emite **una sola línea** con la cuenta y los nombres, nunca una por curso:
   `⚠ [N] cursos sin tocar nunca: [a], [b], [c]…`
   Una alerta que ocupa doce líneas entierra a las once que sí eran urgentes, y a las dos semanas el usuario salta el bloque entero. **El ruido no es un problema estético: es cómo se apaga sola una red de seguridad** — y este bloque, apagado, es justo el fallo del 2026-07-18. Vale para cualquier alerta que pueda cumplirse en muchos cursos a la vez.
3. **Hito vencido** — un curso `hito` cuya `condicion` ya se cumple pero sigue sin tocarse.
   `⚠ [Curso] cumple su condición de arranque y sigue sin empezar.`
4. **Hito urgente activo** — un curso `hito` cuya `condicion_urgente` se cumple.
   `⚠ [Curso] está en condición URGENTE: debería mandar sobre el resto de clases.`
5. **Repasos desbordados** — más de 5 temas con `fecha_proximo_repaso` vencida.
   `⚠ [N] temas con repaso vencido. Se están acumulando más rápido de lo que se cierran.`
6. **Atraso de plan** — `sum(sesiones_dedicadas)` de un curso supera en >50% la suma de sus `sesiones_por_nivel` hasta los niveles requeridos por sus objetivos (el coste estimado de aprenderlo a ese nivel).
   `⚠ [Curso] va [N]% por encima de las sesiones estimadas.`
7. **Latente que debería estar despierto** — un objetivo `latente` cuya `condicion_activacion` ya se cumple.
   `⚠ [Objetivo] cumple su condición de activación y sigue latente. Debería estar compitiendo por horas.`
   Es el hermano del canario de cursos huérfanos: un objetivo dormido de más no se queja solo.
8. **Hito inviable** — algún hito no condicional cuya validación (regla 10, cola con prioridad) ya no cabe.
   `⚠ [Hito] de [Objetivo] no entra para [fecha]: faltan ~[N] horas.`
   Si el hito es de examen universitario, **no es negociable**: la opción no es mover la fecha, es recortar alcance o aflojar otro objetivo.
9. **Objetivo prioritario cediendo** — el objetivo más prioritario (el de `prioridad` MENOR: 1 = lo primero) va atrasado mientras uno menos prioritario va al día.
   `⚠ [Objetivo prioritario] va atrasado y [Objetivo secundario] al día. Considera aflojar la cadencia del secundario.`

   **Tiene DOS versiones. La barata corre en cada sesión; la cara solo en `estado`.**

   **9a — BARATA (PASO 5.5, cada sesión).** Es lookup puro, sin cola de viabilidad. CUENTA: sea `P` el objetivo activo de `prioridad` menor. Si su `_nota_viabilidad` (o cualquier campo de `objetivo.json`) declara un DÉFICIT vigente, y **el curso que ganó el selector NO sirve a `P` por NINGUNA vía** —ni aparece en su `cursos_requeridos` (tronco) ni en la cerradura de prerrequisitos que arrastra la base (`aprender(P) ∪ mantener(P)`, PASO 5.5 del README)—, entonces se cumple. Es decir: estás a punto de gastar horas en un objetivo que NO es `P` ni lo alimenta, mientras `P` ya declaró que no le alcanzan.
   **NO uses solo `cursos_requeridos` como universo de "sirve a P":** un curso base (01-05) puede no estar listado ahí y aun así sostener a `P` entero vía cerradura — es la misma trampa que P9 en `ERRORES.md` (cursos_requeridos vs. cerradura respondiendo la pregunta equivocada). Antes de marcar "no sirve", comprueba la cerradura, no solo el array.
   `⚠ La clase de hoy ([curso]) sirve solo a [objetivo secundario]. [Objetivo prioritario] tiene un déficit declarado de ~[N] h para [hito]. El README manda ofrecer aflojar la cadencia del secundario antes que recortar el prioritario — ¿lo revisamos?`
   **Esta es la que no existía el 2026-07-18 y por la que el agente propuso una clase de aptitud sin mencionar el déficit de 52 h del semestre que tenía leído en su propio contexto.** El detector caro estaba escrito; lo que faltaba era una versión que cupiera en el arranque.

   **9b — CARA (`estado`).** La versión rigurosa: corre la cola con prioridad y determina "va atrasado" de verdad, en vez de fiarse de un déficit declarado a mano que puede estar rancio.

   **Los dos avisos apuntan a la MISMA palanca**, y no te la inventes: es la de "Ceder desde el objetivo menos prioritario" de la sección de Re-planificación del `README.md` (aflojar `cadencia_dias` del objetivo que puede esperar). **Proponla, nunca la apliques por tu cuenta.** Y al aplicarla, deja escrito el bloque `_cadencia_temporal` en el `temario.json` del curso con su `restaurar_cuando` — si no, "temporal" se convierte en permanente por inercia, que es matar el curso con otro nombre.
10. **Horas prometidas que no ocurren** — con ≥3 semanas de historial, `horas_por_semana_real` difiere >30% de la suma declarada.
   `⚠ El plan asume [S] h/semana pero llevas [W] h/semana reales. Todas las proyecciones de deadline están calculadas sobre horas que no están ocurriendo.`
   Es la alerta que solo el historial puede dar. Un plan puede tener la aritmética perfecta y aun así estar validado contra un usuario imaginario.
11. **Ritmo sin medir** — `historial.json` tiene menos de 5 sesiones.
   `⚠ Ritmo real aún sin medir ([N] sesiones). Las viabilidades de arriba asumen 1.5 h/sesión — trátalas como estimaciones, no como hechos.`
12. **Sobre-especificación de curso (jalar un curso entero donde bastan temas)** — un objetivo lista en `cursos_requeridos` un curso C con `nivel_requerido: "base"` (= lo que el cierre ya daría por defecto) y, al mismo tiempo, los OTROS cursos de ese objetivo ya referencian temas de C por `prerrequisitos_externos`. Señal de que C está declarado de más: el cierre transitivo lo traería igual, y **solo los temas que hacen falta**, en vez del curso completo.
   `⚠ [Objetivo] requiere el curso [C] entero a nivel base, pero sus cursos propios ya dependen de [n]/[m] temas de [C] por prerrequisito. Probable sobre-especificación (regla 12 del checklist): considera quitar [C] de cursos_requeridos y dejar que el cierre traiga solo lo necesario.`
   **Excepción legítima, NO la marques:** cuando el objetivo TRATA de C y lo pide a un nivel MAYOR que el default del cierre (ej. la UNI pide toda la base a `avanzado`) — ahí el curso entero sí es lo que el objetivo quiere, no un arrastre. La distinción es el nivel: `base` = redundante con el cierre; `intermedio`/`avanzado` = el objetivo lo trata de verdad.
   Esta alerta existe porque **este proyecto ya cometió ese error dos veces** (el semestre listaba los 6 cursos base enteros); la regla de "jalar temas, no cursos" estaba escrita pero nada la verificaba. Esto la verifica.
13. **Tema que se resiste (profundidad a re-verificar)** — un tema con `veces_repasado` alto (≥4) pero `nivel_repaso` bajo (≤2): se ha repasado muchas veces y aun así no consolida.
   `⚠ [tema] repasado [N] veces y sigue en nivel_repaso [M]. No consolida: su nivel_alcanzado ([nivel]) probablemente ya no es real — re-verifícalo antes de darlo por sabido para cualquier objetivo.`
   El `nivel_alcanzado` NO baja solo (la profundidad no se des-verifica automáticamente), pero este cruce es la señal de que quizás debería: trátalo como candidato a una clase de `repaso` dedicada y a re-comprobar el criterio de dominio. Es justo lo que `nivel_repaso` por sí solo esconde, porque se resetea al fallar y pierde el historial.

14. **Temario inferido sin validar contra el sílabo real** — un curso con `silabo_validado.validado == false` en su `temario.json`, cuyo objetivo ya arrancó (`hoy >= fecha_inicio` del objetivo que lo requiere).
   `⚠ [Curso] sigue con temario INFERIDO y las clases empezaron el [fecha]. Hasta validarlo contra el sílabo real, cada sesión de este curso puede ir sobre contenido que no van a preguntar.`
   **Esta alerta NO es el recordatorio: es el detector.** Quien DISPARA la revisión es Ángela (la agenda externa): tiene el evento de la semana 1 de clases y trae la confirmación por el sync dominical — ver `_protocolos/coordinacion-agenda.md`. Esta alerta existe para el caso en que ese disparador no dispare, o dispare y nadie actúe. Es la misma lógica que el CANARIO del PASO 5.5: *el seguro no es un disparador perfecto; es que un disparador que no dispara se vea.*
   **Cómo se apaga:** al contrastar el temario contra el sílabo real, pon `silabo_validado` en `{"validado": true, "fecha": "YYYY-MM-DD", "fuente": "..."}` y corrige lo que no cuadre. Aprovecha esa misma pasada para resolver los `_nota_opcional` de los temas con `opcional: true`: varios dicen "confirmar en la semana 1 y quitarle el opcional si el sílabo real lo exige".
   Estudiar sobre un temario equivocado es tiempo perdido con aritmética perfecta: el plan cuadra, las horas se cumplen, y el contenido no es el que preguntan.

15. **Auditoría cara vencida** — BARATA, y es la que hace visible que las CARAS no están disparando.
    CUENTA: lee `objetivo.json → ultima_auditoria`. Se cumple si **cualquiera** de las tres:
    (a) `hoy - ultima_auditoria.fecha > ultima_auditoria.vence_a_los_dias`;
    (b) el campo **no existe** (nunca se auditó);
    (c) **`ultima_auditoria.invalidada_por` no es `null`** — alguien detectó que el cálculo se hizo sobre un modelo que después resultó equivocado. **Este caso es MÁS urgente que el vencimiento por fecha**, porque las cifras no están viejas: están mal, y siguen pareciendo válidas.
    `⚠ La última auditoría de viabilidad es del [fecha] ([N] días). Las alertas CARAS (8, 9b, 12) no se han evaluado desde entonces, y las cifras de déficit que uses hoy vienen de ahí. Corre 'estado' para refrescarlas.`
    `⚠ La auditoría del [fecha] está INVALIDADA: [invalidada_por]. No uses sus cifras de déficit como firmes hasta recalcular.`
    **CUANDO ESTA ALERTA SE CUMPLE, NO TE LIMITES A AVISAR: CORRE LA PARTE CARA AHÍ MISMO.** Es la automatización parcial de `estado`, y es segura porque **se auto-limita**: en cuanto corre, actualiza `ultima_auditoria.fecha` y no vuelve a dispararse hasta dentro de 21 días (o hasta que algo la invalide). Como mucho, una vez cada tres semanas.

    Qué correr exactamente: la **cola de viabilidad** de los objetivos ACTIVOS (sus hitos no condicionales por fecha) + las alertas **8, 9b y 12**. No hace falta el informe completo de `estado` — solo la partida cara. Repórtalo en pocas líneas y **sigue con la clase**: esto no cancela la sesión de estudio, la informa.

    **Por qué automático y no "¿quieres que la corra?":** una pregunta que el usuario puede contestar "ahora no" reintroduce el patrón P1 por la puerta de atrás — vuelve a depender de que alguien decida acordarse, justo cuando está a punto de ponerse a estudiar y lo último que quiere es un desvío. El coste de correrla sin preguntar es unos minutos cada tres semanas; el coste de no correrla es planificar un semestre entero sobre cifras falsas. **Lo que sí se pregunta es qué hacer con el resultado, no si mirarlo.**

    **División del trabajo, para que quede claro qué está automatizado y qué no:**
    - **Cada sesión, automático:** todas las alertas BARATAS (PRE-FLIGHT, regla 6 del PASO 5.5).
    - **Cada ~21 días o al invalidarse, automático:** la partida CARA, disparada por esta alerta.
    - **A mano:** solo el informe completo de `estado` cuando el usuario quiere ver la foto entera. Ya no es la única vía para que las alertas caras existan.

    Esta alerta es el corazón del arreglo del 2026-07-18. No hace el trabajo caro por sí misma: hace que **no hacerlo se vea** — y ahora, además, que se haga. Es la misma lógica del CANARIO y de la alerta 14 — *el seguro no es un disparador perfecto; es que un disparador que no dispara se vea.* Sin ella, "corre `estado` cada 2-3 semanas" es una sugerencia sin memoria: nadie sabe cuándo fue la última vez.
    **Cómo se apaga:** corriendo `estado`, que al terminar DEBE actualizar `ultima_auditoria.fecha` a hoy. Ese es el único escritor de ese campo.

18. **Cambio de regla sin simulacro registrado** — BARATA. **Es el verificador de la Rama 2C, y no depende de que nadie se acuerde de nada.**
    CUENTA: en `VERSIONES.md`, toma la **versión más reciente** (el primer bloque `## v...`). Cuenta sus viñetas de "Añadido" y "Cambiado" que **no terminan** con un marcador literal `[SIM: YYYY-MM-DD]` o `[SIM: no aplica — <razón>]`. Si son ≥ 1, se cumple.
    `⚠ [N] cambio(s) de la versión [v] sin simulacro registrado: [primeras palabras de cada uno]. Se escribieron pero nunca se ejecutaron contra el estado real.`
    **Por qué existe (2026-07-18, señalado por el usuario).** La Rama 2C se saltó cuatro veces en un día pese a estar escrita. El primer intento de arreglo fue exigir que el agente emitiera la tabla del simulacro en su reporte — pero eso deja el **verificador en la atención del usuario**: tiene que recordar, durante meses y sobre decenas de funcionalidades, que las tablas deben aparecer. **Un control cuyo verificador es la memoria humana es el patrón P1 con otro traje**, y este proyecto existe precisamente porque la memoria es frágil. El marcador `[SIM:]` mueve la prueba **al repo**, donde una alerta barata la comprueba sola en cada arranque.
    **La tabla en el reporte sigue siendo obligatoria** (Rama 2C) — es lo que el usuario ve en el momento. Esta alerta es lo que lo caza **después**, y es la que de verdad no se puede burlar.
    **Cómo se apaga:** corriendo el simulacro y añadiendo su marcador. Poner `[SIM: no aplica]` sin razón escrita cuenta como no puesto.

17. **Errores sin analizar en la bandeja** — BARATA.
    CUENTA: en `ERRORES.md`, dentro de la sección "Bandeja de entrada", cuenta **solo las líneas de lista que empiezan literalmente por `` - `[SIN ANALIZAR]` ``** (guion, espacio, la marca entre backticks, al principio de la línea). Si son ≥ 1, se cumple.
    **NO cuentes menciones en prosa.** La cabecera de esa sección y este mismo protocolo explican qué es la marca y la nombran; contarlas hace que la alerta dispare sobre su propia documentación con la bandeja vacía. *(Falso positivo real, cazado por el simulacro de la Rama 2C el 2026-07-18, el mismo día en que se escribió la alerta.)*
    `⚠ Hay [N] error(es) detectado(s) y sin analizar en ERRORES.md, el más viejo del [fecha]. Se anotaron en bruto para no cortar una clase; siguen esperando su patrón y su arreglo.`
    **Por qué existe:** el procedimiento de errores permite anotar en bruto un fallo detectado a mitad de una clase, para no romper el estudio. Sin algo que recoja esas notas después, son texto que el usuario olvida y el agente olvida — el patrón P1 cometido dentro del archivo que lo documenta. Esta alerta es lo que las mantiene vivas.
    **Cómo se apaga:** procesando el caso (convertirlo en caso de un patrón o en patrón nuevo) y **borrando su línea de la bandeja**. La bandeja vacía es el estado normal; que crezca significa que se detectan errores más rápido de lo que se analizan.

16. **Cadencia temporal vencida** — BARATA.
    CUENTA: por cada `temario.json` con un bloque `_cadencia_temporal`, evalúa su `restaurar_cuando`. Si se cumple, se cumple la alerta.
    `⚠ [Curso] tiene la cadencia relajada a [N] días desde el [fecha] y su condición de restauración ya se cumple ([condición]). Toca devolverla a [valor_original].`
    Existe porque **una medida temporal sin verificador se vuelve permanente por inercia**, y eso convierte "relajar" en "matar" sin que nadie lo decida. El bloque `_cadencia_temporal` lleva su propia condición de retiro justo para que esta alerta pueda leerla sin juicio.

**Correr `estado` cada 2-3 semanas ya NO depende de que el usuario se acuerde:** la alerta 15 lo reclama sola en cuanto la auditoría vence. Sigue siendo barato, no arranca nada, y es el único momento en que el proyecto corre su auditoría **completa** — pero ahora su ausencia es visible en vez de silenciosa.

---

## Comando `backup`

Genera un ZIP del proyecto completo (excepto `_backups/`) con nombre `YYYY-MM-DD-HHMM.zip` dentro de la carpeta `_backups/`.

### Cuándo sugerirlo proactivamente

- Antes de invocar el protocolo de expansion (agregar nuevo objetivo).
- Antes de eliminar un curso a pedido del usuario.
- Antes de procesar un handoff de VUELTA si el usuario lo pide.

En estos casos, ofrece hacer un backup — no lo ejecutes sin permiso.

### Pasos del agente

1. Verifica si tienes capacidad de ejecutar comandos de sistema:
   - **Sí (Claude Code y similares):** procede al paso 2.
   - **No (agente sin shell):** avisa al usuario e indícale que puede correr manualmente en PowerShell:
     ```
     Compress-Archive -Path * -DestinationPath "_backups\$(Get-Date -Format 'yyyy-MM-dd-HHmm').zip" -Exclude "_backups"
     ```
     O el equivalente en su sistema.

2. Crea `_backups/` si no existe.

3. Genera el nombre del archivo con timestamp: `YYYY-MM-DD-HHMM.zip`.

4. Ejecuta el compresor apropiado:
   - **Windows PowerShell:** `Compress-Archive -Path * -DestinationPath "_backups\<nombre>.zip" -Exclude "_backups"`
   - **Bash (Linux/Mac):** `zip -r "_backups/<nombre>.zip" . -x "_backups/*"`

5. Verifica que el ZIP se creó y reporta al usuario:
   ```
   Backup generado:
     Archivo: _backups/YYYY-MM-DD-HHMM.zip
     Tamaño: X KB
     Contenido: N archivos

   Recomendación: si usas git+GitHub, hacer commit y push también protege este estado
   con historial y remoto. El backup local es complemento, no reemplazo.
   ```

6. **Política de retención:** no borres backups antiguos automáticamente. Si el usuario ve que `_backups/` crece mucho, que él decida qué eliminar (o considera sugerirle cambiar a git).

---

## Notas sobre respaldo del proyecto

**Recomendación primaria:** usar git + GitHub (o cualquier remoto). Ventajas frente al ZIP:
- Historia completa de cambios (diff por commit).
- Restauración selectiva de archivos.
- Remoto gratuito y accesible desde cualquier PC.
- Colaboración si en el futuro varias personas usan el mismo proyecto.

Si el usuario no está familiarizado con git, este proyecto se puede inicializar como repositorio con `git init` y luego pushear a un repo privado en GitHub. Cada cierre de clase importante puede terminarse con un `git commit` para dejar snapshot de progreso.

**Cuándo usar `backup` (ZIP local):**
- Usuario no quiere usar git.
- Necesita snapshot instantáneo antes de una operación grande.
- No tiene internet en ese momento.

Los dos mecanismos pueden coexistir.
