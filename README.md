# Plan de Estudios

Repositorio de estudio para retomar cursos de forma estructurada, usado junto con un agente de IA (Claude Code, GPT, agente local, etc.) como tutor. Este proyecto es una **plantilla**: no asume qué quieres aprender. En el primer arranque, el agente entrevista al usuario y genera todo el plan personalizado (cursos, temarios, bibliografía, timeline).

---

> # 🚧 MIGRACIÓN ABIERTA — LEE ESTO PRIMERO
>
> **Existe [`PENDIENTE.md`](PENDIENTE.md) en la raíz. Léelo ANTES de tocar nada.**
>
> El 2026-07-16 se rediseñó el núcleo del proyecto (el conocimiento pasó a medirse en dos dimensiones: profundidad y durabilidad). **Los datos ya usan el modelo nuevo, pero parte de ESTE README todavía describe el viejo y se contradice a sí mismo.** `PENDIENTE.md` dice qué parte manda y qué falta cerrar.
>
> Lo más importante que debes saber: **`nivel_alcanzado` es el campo central del modelo y hoy no lo escribe nadie**, porque el "Protocolo al CERRAR una clase" aún no sabe que existe. Hasta que eso se arregle, el rediseño es decorativo.
>
> *Borra este aviso y el archivo cuando la migración esté cerrada.*

---

# ⚠️ INSTRUCCIONES OBLIGATORIAS PARA EL AGENTE DE IA

> **Si eres un agente de IA leyendo este README para asistir al usuario, ejecuta el siguiente algoritmo ANTES de responder cualquier cosa. Los pasos son estrictamente secuenciales.**

## Algoritmo de arranque (ejecutar SIEMPRE al inicio de la sesión)

**Palabra explícita `empezar`** (alias corto: `emp`)**:** este algoritmo debe ejecutarse siempre al inicio de la sesión, sin que el usuario tenga que pedirlo. Pero como en la práctica el agente necesita un primer mensaje del usuario para actuar (incluso con este archivo cargado como contexto), `empezar` es la forma explícita y sin ambigüedad de decir "ejecuta el algoritmo de arranque ahora" — útil como primer mensaje al abrir una sesión nueva cuando el usuario no sabe qué escribir, o en cualquier momento para forzar una relectura completa del estado del proyecto (ej. tras cambiar de agente, o si el contexto de la conversación se perdió). Si el usuario escribe `empezar` o `emp`, ejecuta este algoritmo completo desde el PASO 1 de inmediato.
>
> **Nota — convención de comandos sin barra, en todo el proyecto:** ningún comando de este proyecto (arranque, configuración, utilidades, handoff) empieza con `/`. Herramientas como Claude Code interceptan los comandos que empiezan con `/` a nivel de la propia interfaz, antes de que el mensaje le llegue al agente como texto — así que un comando con `/` definido aquí podría nunca llegar a activarse, o chocar con un comando propio de la herramienta (ej. el `/init` interno de Claude Code, que hace algo completamente distinto: generar un archivo `CLAUDE.md`). Por eso, en vez de barras, cada comando tiene una **forma completa** en palabras (frase natural, ej. "mostrar estado") y un **alias corto** en forma de acrónimo intuitivo sin barra (ej. `estado`, `hndff`, `ucs on`). Esto evita el choque ahora y ante cualquier comando nuevo que la herramienta agregue en el futuro. Ver la tabla completa de comandos y alias en `_protocolos/configuracion.md`, `_protocolos/utilidades.md` y `_protocolos/handoff.md`.

```
PASO 1 — ¿Existe el archivo "objetivo.json" en la raíz del proyecto?

    ├── NO existe
    │     → Estás en modo SETUP INICIAL.
    │     → Lee y sigue paso a paso: _protocolos/entrevista.md
    │     → NO leas nada más de este README hasta terminar el setup.
    │     → FIN del algoritmo.
    │
    └── SÍ existe
          → Continúa al PASO 2.

PASO 2 — Lee "objetivo.json". Recorre "objetivos" (es un ARRAY:
         puede haber varios en paralelo, cada uno con su "estado" —
         ver "Objetivos paralelos" más abajo).

         2a. DESPIERTA LOS LATENTES. Por cada objetivo con
             estado "latente", evalúa su "condicion_activacion"
             contra el estado real del proyecto. Si se cumple:
             cámbialo a "activo", DÍSELO al usuario en una línea
             ("Se activó el objetivo X: ya rendiste el examen UNI")
             y sigue.
             Un objetivo latente que cumple su condición y nadie
             despierta es el mismo bug que un curso huérfano: existe,
             debería estar corriendo, y no se queja. Este chequeo es
             lo único que lo impide.

         2b. ¿Algún objetivo "activo" tiene "estado_setup" distinto
             de "completo"?

    ├── SÍ, alguno está incompleto
    │     → El setup de ese objetivo quedó a medias.
    │     → Lee _protocolos/entrevista.md y retoma desde donde quedó,
    │       trabajando SOBRE ESE objetivo (no toques los demás).
    │     → FIN del algoritmo.
    │
    └── NO, todos los activos están completos
          → Continúa al PASO 3.

PASO 3 — ¿El usuario está invocando un COMANDO RÁPIDO
         (configuración o utilidad)?
         Se activa SOLO si escribe forma completa O alias corto de un
         comando listado en:
           - _protocolos/configuracion.md:
                 "mostrar configuracion"                 (alias: cfg)
                 "activar modo una clase por sesion"     (alias: ucs on)
                 "desactivar modo una clase por sesion"  (alias: ucs off)
           - _protocolos/utilidades.md:
                 "mostrar estado"                        (alias: estado)
                 "generar backup"                        (alias: backup)

    ├── SÍ
    │     → Lee el protocolo correspondiente y aplica la acción.
    │     → Confirma al usuario con una línea corta.
    │     → FIN del algoritmo (no continúes con más pasos).
    │
    └── NO
          → Continúa al PASO 4.

PASO 4 — ¿El usuario está invocando el protocolo de handoff?
         Se activa SOLO si:
           (a) El usuario escribe un comando de _protocolos/handoff.md
               en su forma completa O como alias corto
               (hoy: "generar un handoff" / alias "hndff")
           O
           (b) El usuario pega un bloque cuya PRIMERA línea es
                 === HANDOFF-RESUMEN ===

    ├── SÍ
    │     → Lee _protocolos/handoff.md y aplica la FASE
    │        correspondiente (SALIDA para (a), VUELTA para (b)).
    │     → FIN del algoritmo.
    │
    └── NO
          → Si el usuario dijo algo PARECIDO pero no idéntico ni al
            comando completo ni al alias corto, NO invoques el
            protocolo — pregunta si quiere usar la forma exacta y espera.
          → Continúa al PASO 5.

PASO 5 — ¿El usuario está pidiendo agregar un NUEVO objetivo distinto
         a los activos, o los cursos de algún objetivo activo están
         todos al 100%?

    ├── SÍ
    │     → Lee _protocolos/expansion.md y sigue ese protocolo.
    │     → FIN del algoritmo.
    │
    └── NO
          → Es una sesión normal de estudio.
          → Continúa al PASO 5.5.

PASO 5.5 — SELECTOR DE CLASE. Decide QUÉ CLASE toca hoy.

         Este paso corre SIEMPRE en toda sesión normal, antes de
         cualquier contenido. No lo saltes aunque el usuario diga
         "sigamos con X" — primero calcula, luego propón.

         Es un paso SIN MEMORIA: se recalcula desde los archivos en
         cada sesión. Por eso funciona igual si el usuario estudia
         una vez al día o tres (ver "Varias sesiones el mismo día"
         más abajo).

         Lee objetivo.json y recorre los cursos de TODOS los objetivos
         con estado "activo" (cada uno tiene su cursos_requeridos).
         Los objetivos "latente" y "completado" NO compiten.

         UN CURSO PUEDE ESTAR EN VARIOS OBJETIVOS. No busques "su"
         dueño — no existe. Calcula su URGENCIA EFECTIVA:

             urgencia_efectiva(curso) = la del objetivo ACTIVO más
             urgente que lo requiere.
             "Más urgente" = menor "prioridad" ordinal; si empatan,
             el de hito sin cumplir más cercano en fecha.

         De ese objetivo salen la fecha y la prioridad que usarás
         para ese curso. Esto es lo que impide que un curso base
         compartido (trigonometría) herede la fecha lejana del
         objetivo que lo creó cuando otro lo necesita YA.

         De cada curso lee:
           - temario.json  → campo "modo_estudio" (tipo, cadencia_dias,
                             prioridad, condicion, condicion_urgente)
           - progreso.json → estado, ultima_sesion,
                             fecha_proximo_repaso, nivel_repaso

         Evalúa estas ramas EN ORDEN. La PRIMERA que aplique gana:

         (A) CONTINUACIÓN — ¿hay algún tema con estado "en_progreso"?
               → La clase es ese tema. Cerrar lo abierto siempre le
                 gana a abrir algo nuevo.

         (B) HITO URGENTE — ¿algún curso "hito" cumple su
             "condicion_urgente"? (típicamente: se acerca el deadline)
               → Propón esa clase. Va ARRIBA de repaso y cadencia a
                 propósito: en el endgame, el simulacro ES el repaso —
                 revela los vacíos reales mejor que cualquier otra cosa.

         (C) REPASO — ¿hay 3 o más temas con fecha_proximo_repaso <= hoy,
             O algún tema vencido por más del DOBLE de su intervalo?
               → Clase de repaso dedicada: sin tema nuevo, ejercicios
                 mezclados de los temas vencidos.
               → Es el escape hatch. En condiciones normales el repaso
                 NO se hace así, sino intercalado dentro de la clase
                 de tema nuevo (ver Método 2 y Método 3).

         (D) CADENCIA — ¿algún curso con modo_estudio.tipo == "cadencia"
             lleva >= cadencia_dias desde su última sesión?
               (última sesión del curso = el ultima_sesion más reciente
                entre sus temas en progreso.json; si todos son null,
                el curso nunca se tocó y cuenta como vencido)
               → Clase de ese curso.
               → Si hay varios vencidos, gana el de MENOR "prioridad"
                 (es ordinal: 1 = lo primero. Ver "Cómo se estudia
                 cada curso").
                 Si empatan en prioridad, MUESTRA las opciones al
                 usuario y que elija él.

         (E) HITO NORMAL — ¿algún curso "hito" cumple su "condicion"
             (la no urgente)?
               → Propón esa clase.

         (F) TEMA NUEVO — ninguna de las anteriores aplicó. Caso normal.
               → Próximo tema "no_iniciado" de un curso "ruta" cuyos
                 prerrequisitos internos ya tengan nivel suficiente:
                 su nivel_alcanzado NO es null (llegaron al menos a
                 "base"). No exijas "avanzado" para arrancar el que
                 sigue — un prerrequisito en "base" ya sostiene el
                 tema nuevo; su profundidad definitiva se termina de
                 subir cuando el objetivo que la pide lo cobre. Si un
                 prerrequisito sigue en null, ese es el que toca, no
                 este.
               → Si hay varios candidatos, gana el de MENOR "prioridad"
                 de curso (1 = lo primero). Si sigue sin estar claro,
                 pregunta.

         REGLAS DE ESTE PASO (no las rompas):

           1. PROPONES, NO IMPONES. Anuncia en UNA línea qué clase toca
              y por qué ("Toca aptitud: 5 días sin tocarla"). Si el
              usuario quiere otra cosa, se hace lo que él diga, sin
              insistir. Su vida real manda sobre el JSON.

           2. NO HARDCODEES CURSOS. Este paso no sabe que "aptitud" o
              "inglés" existen: solo recorre los cursos y lee su
              "modo_estudio". Un curso nuevo entra sin tocar este paso.

           3. Si ya se cerró un tema nuevo HOY y el usuario abre otra
              sesión, MENCIONA que una segunda sesión rinde más como
              cadencia o repaso que como un segundo tema nuevo. Solo
              mencionar — él decide.

           4. No inventes tipos de clase fuera de (A)-(F).

           5. DOS DESEMPATES, EN ORDEN. Cuando dos candidatos compiten
              dentro de la misma rama, primero gana el de URGENCIA
              EFECTIVA mayor (definida arriba); solo si empatan, gana
              la "prioridad" del curso dentro de su temario (1 = lo
              primero). Si siguen empatados, pregunta al usuario.
              Las condiciones de deadline de un curso "hito" se
              evalúan contra los hitos del objetivo que le da su
              urgencia efectiva.

           6. CANARIO — ANTES de anunciar la clase elegida, revisa si
              algún curso está siendo IGNORADO por el sistema:
                - un curso "cadencia" vencido por más de 3× su
                  cadencia_dias, o
                - un curso "hito" cuya condición ya se cumple pero que
                  sigue sin tocarse (todos sus ultima_sesion en null).
              Si encuentras alguno, DILO en una línea aunque otra rama
              haya ganado: "Aviso: simulacros cumple condición hace 3
              semanas y sigue sin tocarse."
              Esto existe porque un disparador que falla no avisa de
              que falló. Es la red de seguridad de este paso entero.
              Ver también el comando `estado` en _protocolos/utilidades.md.

           7. PREDICCIÓN DE OLVIDO — si hay un hito no condicional a la
              vista (digamos, dentro de las próximas ~4 semanas), revisa
              los temas que ese hito cubre y avisa de los que van a
              LLEGAR FRÍOS: aquellos cuyo nivel_repaso no retiene la
              distancia que falta hasta la fecha (retención(nivel_repaso)
              < días de hoy al hito). DILO en una línea aunque otra rama
              haya ganado: "Aviso: factorización llega al parcial en
              nivel_repaso 2 (retiene ~3 días) y el examen está a 12
              días de su último repaso: va a estar olvidada." Es lo
              único que distingue un plan que cuenta horas de uno que
              predice resultados (ver "La durabilidad"). No lo omitas
              por no dar malas noticias; el cálculo completo por hito
              vive en el comando `estado`.

    → Continúa al PASO 6 con el tipo de clase ya decidido.

PASO 6 — Ejecutar la clase decidida en el PASO 5.5. Antes de arrancar:
          - Lee configuracion.json (si no existe, créalo con
            defaults según _protocolos/configuracion.md).
          - Si "una_clase_por_sesion" es true, anuncia al usuario qué
            clase se trabajará y que la sesión se cierra cuando la
            clase cierre (ver el detalle en
            _protocolos/configuracion.md).
         Luego aplica:
          (a) "Protocolo al INICIAR una sesión o clase" (más abajo)
          (b) Los "Métodos de estudio" (más abajo)
          (c) Al terminar, aplica "Protocolo al CERRAR una clase"
          (d) Si "una_clase_por_sesion" está activo, recuérdale al
              usuario al cerrar que abra una nueva sesión para la
              próxima clase.
```

### Reglas irrompibles

1. **NUNCA te saltes el PASO 1**. Aunque el usuario diga "vamos a estudiar X directamente", primero verifica si existe `objetivo.json`.
2. **NUNCA generes contenido de estudio si algún objetivo `activo` tiene `estado_setup ≠ "completo"`**. Termina primero ese setup.
3. **SIEMPRE lee los archivos JSON del curso antes de dar una clase** (`temario.json`, `progreso.json`, `dificultades.json`, `glosario.json`). No confíes en la memoria de la conversación previa — puede ser una sesión nueva con otro agente.
4. **SIEMPRE actualiza los archivos JSON al cerrar la clase**. Es la única forma de que la siguiente sesión (con este agente u otro) sepa dónde quedaste.
5. **NUNCA te saltes el PASO 5.5**. Aunque el usuario diga "sigamos con aritmética", primero calcula qué clase toca y dilo en una línea. Él puede ignorarte y seguir con lo que quiera — pero tiene que poder decidir con la información delante, no a ciegas.

---

## Anatomía de una clase

> Esta sección define las unidades del proyecto. El PASO 5.5, la configuración y el protocolo de handoff dependen de ella.

**Sesión** = un chat con el agente, de principio a fin.
**Clase** = la unidad de trabajo del día. Una sesión contiene **una clase**.

Una clase tiene **como máximo un tema nuevo**, y alrededor de él puede contener repaso, ejercicios intercalados de temas ya cerrados, y lo que haga falta. Nada de eso cuenta como "cambiar de tema": es la misma clase.

Lo que la regla `una_clase_por_sesion` protege es **no abrir un segundo frente nuevo antes de cerrar el de hoy** — y que el chat no se estire una semana entera acumulando historial. No restringe qué archivos puede leer el agente dentro de la clase: si para intercalar bien hace falta abrir el README de una clase vieja, se abre.

### Contención del alcance de la clase

La regla de arriba evita abrir un segundo tema **a propósito**. Pero el modo real en que una clase se descontrola es otro, y hay que nombrarlo aparte: **la clase crece sola**. Sale una duda, la duda abre un subtema, el subtema abre otro, y tres horas después se estudió de todo y no se cerró nada. Nadie decidió eso; simplemente pasó. Es la deriva por curiosidad, y es la falla más común de estudiar con un agente, porque el agente siempre tiene una respuesta más que dar.

Cuando en medio de una clase aparezca algo que no es necesario para el tema de hoy, clasifícalo **antes** de perseguirlo:

1. **¿Es un prerrequisito realmente roto que bloquea el tema de hoy?** → Atiéndelo. Es parte legítima de la clase y no es deriva. Regístralo en `dificultades.json`.
2. **¿Es un tema que ya está en el temario, más adelante?** → **No lo estudies ahora.** Dilo en una línea ("eso es `05-factorizacion`, lo vemos cuando toque") y sigue. El temario ya decidió cuándo va; el orden existe por algo.
3. **¿Es una curiosidad legítima pero fuera de la ruta al objetivo?** → Contéstala en 2-3 frases, sin abrir clase, y sigue. No la conviertas en un desvío de media hora.
4. **¿Es un hueco de base que no bloquea hoy pero va a estorbar después?** → Anótalo en `dificultades.json` con `resuelto: false` y sigue. Va a salir solo en una sesión futura por la vía del repaso.

Además, **avisa cuando la clase se esté pasando de largo**. Si la sesión ya cubrió los `objetivos_aprendizaje` del tema, o ya lleva claramente más de lo que sugieren sus `sesiones_por_nivel` hasta el nivel que toca hoy, dilo y ofrece cerrar: "Ya cubrimos los objetivos del tema. ¿Cerramos aquí y dejamos lo demás para la próxima?" No decidas tú cerrar — pero tampoco dejes que la clase se estire tres horas sin que nadie lo mencione. **Terminar el tema de hoy vale más que cubrir cinco temas a medias**, y una clase cerrada limpia es la única que deja el `progreso.json` en un estado del que la siguiente sesión puede partir.

### Tipos de clase

| Tipo | Qué es | Quién lo dispara |
|---|---|---|
| `continuacion` | Retomar un tema que quedó `en_progreso` | Rama (A) del PASO 5.5 |
| `hito` | Clase que se dispara por condición (simulacros) | Ramas (B) urgente y (E) normal |
| `repaso` | Sin tema nuevo; ejercicios mezclados de temas vencidos | Rama (C) — escape hatch |
| `cadencia` | Clase de un módulo recurrente (aptitud, idiomas) | Rama (D) |
| `tema-nuevo` | Caso normal: un tema nuevo + repaso intercalado | Rama (F) |

### Cómo se estudia cada curso: `modo_estudio`

Cada `temario.json` declara, a nivel de curso, **cómo se estudia**. El PASO 5.5 lee esto y nada más — no conoce los cursos por nombre.

```json
"modo_estudio": {
  "tipo": "ruta",
  "cadencia_dias": null,
  "prioridad": 1,
  "condicion": null,
  "condicion_urgente": null,
  "nota": "Por qué este curso se estudia así"
}
```

- **`tipo: "ruta"`** — progresión lineal, tema tras tema. Es el modo por defecto y alimenta las clases de `tema-nuevo`. `cadencia_dias` y las condiciones van en `null`.
- **`tipo: "cadencia"`** — módulo recurrente en dosis pequeñas y constantes. Requiere `cadencia_dias` (entero): se propone cuando pasaron ese número de días o más desde su última sesión.
- **`tipo: "hito"`** — se dispara por condición, no por reloj. Tiene **dos** condiciones y las dos son obligatorias:
  - **`condicion`** — cuándo el curso *entra en rotación* (rama E, prioridad baja).
  - **`condicion_urgente`** — cuándo el curso *pasa a mandar* (rama B, por encima de repaso y cadencia). Suele ser una condición de deadline.
- **`prioridad`** — entero **ordinal: 1 = lo primero, 2 = lo segundo, etc. GANA EL MENOR.** Desempata cuando dos cursos compiten el mismo día. Si empatan, el agente pregunta. Se lee como el orden en que se atienden, igual que "primero esto, luego lo otro" — no como un peso donde más es más.

**Las condiciones se escriben en prosa pero deben ser ARITMÉTICA VERIFICABLE, no juicio.** Un agente distinto en una sesión distinta tiene que llegar al mismo resultado. Escribe la cuenta exacta: qué se cuenta, sobre qué universo, y el umbral. Mal: `"cuando ya se cubrió lo suficiente"`. Bien: `"cuenta los temas con opcional:false de todos los cursos con modo_estudio.tipo == 'ruta'; si los que están en estado 'completado' son ≥70% de ese total, se cumple"`.

**Ninguna condición es infalible, y por eso existe la regla 5 (CANARIO) del PASO 5.5 y el bloque de alertas del comando `estado`.** El seguro no es un disparador perfecto: es que un disparador que no dispara se vea. Un curso que existe y nunca se estudia es el bug que este proyecto tuvo desde su creación hasta julio de 2026 — y lo que lo hizo grave no fue el disparador ausente, sino que nada lo hacía visible.

**Para agregar un módulo nuevo** (inglés, lo que sea): se genera su curso normal y se le pone `modo_estudio.tipo: "cadencia"` con su `cadencia_dias`. El PASO 5.5 lo recoge automáticamente. No se toca el algoritmo. Un curso nuevo distinto al objetivo actual pasa por `_protocolos/expansion.md`.

### Objetivos paralelos

> El usuario puede perseguir **varios objetivos a la vez** que no tienen nada que ver entre sí (ej: aprobar un semestre universitario, preparar un examen de admisión *y* aprender un idioma para otro trabajo). `objetivo.json` tiene por eso un array `objetivos`, cada uno con su `estado` — no un objetivo único.

**Hay DOS ejes independientes, y confundirlos es un error que este proyecto ya cometió.** No los mezcles:

| | Pregunta que responde | Dónde vive |
|---|---|---|
| **`modo_estudio`** | ¿*Cómo* se estudia este curso? (lineal / en dosis / por hito) | `temario.json` del curso |
| **Objetivo** | ¿*Para qué* sirve, con qué urgencia y para cuándo? | `objetivo.json`, en el `cursos_requeridos` de cada objetivo (muchos-a-muchos) |

Y hay un **tercer eje** que tampoco hay que mezclar con estos: los **hitos** de un objetivo (¿para cuándo, y qué entra en cada fecha?). Un objetivo puede tener varias fechas de corte que cubren contenido distinto — el parcial de un semestre cubre las semanas 1-8 y el final las 8-16, y no son acumulativos. Ver "Hitos".

Son ortogonales de verdad. Las cuatro combinaciones existen y todas son coherentes:

|  | Objetivo "entrar a la UNI" | Objetivo "conseguir mejor trabajo" |
|---|---|---|
| **`ruta`** | Aritmética, Álgebra, Física… | *(p. ej. un curso con sílabo lineal)* |
| **`cadencia`** | Aptitud Académica | Inglés |

Fíjate en la trampa: **Inglés es `cadencia` porque un idioma se entrena en dosis frecuentes, NO porque sea un objetivo paralelo.** Son dos hechos distintos sobre el mismo curso. Si mañana entra un curso de sílabo lineal para el objetivo de trabajo, será `ruta` *y* paralelo a la vez. Nunca inventes un `modo_estudio` que signifique "es de otro objetivo" — eso ya lo dice `objetivo.json`.

### Cursos compartidos: un curso puede servir a varios objetivos

`cursos_requeridos` es **muchos-a-muchos**. Un curso puede aparecer en varios objetivos, y eso no es una anomalía: es lo normal. Trigonometría sirve al examen UNI *y* a aprobar Física III del semestre.

**No busques el "dueño" de un curso: no existe.** Lo que existe es su **urgencia efectiva** = la del objetivo activo más urgente que lo requiere (menor `prioridad`; si empatan, el de hito pendiente más cercano). De ahí salen su fecha y su prioridad.

> **Por qué importa (bug real, 2026-07-16):** el modelo anterior daba a cada curso un dueño único, y de él heredaba fecha y prioridad. Con trigonometría "perteneciendo" al objetivo UNI (febrero, prioridad 2) pero necesitándose para un parcial de septiembre (prioridad 1), el PASO 5.5 la habría pospuesto justo cuando era urgente. Un campo estaba respondiendo dos preguntas: "¿quién lo creó?" y "¿quién lo necesita y para cuándo?". Solo la segunda importa.

**Estudiar un curso compartido una vez avanza todos los objetivos que lo piden.** Es una inversión, no un gasto duplicado. Y de ahí sale la regla del presupuesto:

### El presupuesto: una línea de tiempo, no un número

**`presupuesto_horas` es un ARRAY de periodos**, no un escalar:

```json
"presupuesto_horas": [
  { "desde": "2026-07-16", "hasta": "2026-08-09", "horas_semanales": 35, "nota": "vacaciones" },
  { "desde": "2026-08-10", "hasta": "2026-11-28", "horas_semanales": 15, "nota": "semestre: ~30 h/sem de clases encima" },
  { "desde": "2026-11-29", "hasta": null,         "horas_semanales": 20, "nota": "post-semestre" }
]
```

> **Nota de cambio (2026-07-16):** existió `horas_semanales_disponibles: 20`, un número único. Se detectó que las 10.6 semanas hasta el parcial eran 3.6 de vacaciones (alta disponibilidad) + 7.0 de semestre (con 30 h/semana de clases encima). **Un escalar promedia dos mundos distintos y miente en ambas direcciones**: infla las semanas de clase e infravalora la ventana que salva el plan. En este caso el error era de 14 h. Si ves `horas_semanales_disponibles` en un archivo viejo, es deuda.

**Las horas disponibles entre hoy y una fecha se INTEGRAN sobre los periodos**, no se multiplican:

```
horas_disponibles(hasta_fecha) =
    Σ  (solape del periodo con [hoy, hasta_fecha]) en semanas × periodo.horas_semanales
```

Un periodo con `"hasta": null` se extiende indefinidamente. Y **todos estos valores son promesas del usuario, no mediciones**: contrástalos con las horas reales de `historial.json` en cuanto haya 3 semanas de datos (ver regla 10).

### El conocimiento se mide en DOS dimensiones, y ninguna es "completado"

> Esta sección es el corazón del modelo. Léela entera antes de tocar `progreso.json`.

Un tema no está "hecho" o "no hecho". Tiene **dos coordenadas independientes**, y confundirlas es lo que hace que un plan mienta:

| Dimensión | Qué mide | Dónde vive | Escala |
|---|---|---|---|
| **Profundidad** | Hasta dónde llegaste | `nivel_alcanzado` en `progreso.json` | `null` → `base` → `intermedio` → `avanzado` |
| **Durabilidad** | Cuánto va a aguantar | `nivel_repaso` en `progreso.json` | `0` → `1` → `2` → `3` → `4` → `5` |

Son **ortogonales**. Un tema puede estar aprendido hondo y sin repasar (se desvanece) o aprendido superficial y repasado hasta arriba (aguanta, pero sigue siendo superficial). Necesitas las dos para saber si un tema estará vivo y suficiente el día del examen.

**`nivel_alcanzado` es un ESTADO VERIFICABLE, no un contador de esfuerzo.** Solo sube cuando el usuario cumple el `criterio_dominio` de ese nivel. Si estudia tres sesiones y sigue fallando, **el nivel no sube** — porque no ha aprendido, y el plan tiene que saberlo. Un contador de sesiones diría "pagado, siguiente" y estaría mintiendo.

> **Nota de cambio (2026-07-16):** existió un `pagado[tema] = sesiones` que llevaba la cuenta del esfuerzo gastado. Se eliminó: **el esfuerzo no es verificable y el estado sí.** El usuario lo señaló y tenía razón. Si ves un modelo que contabiliza sesiones para decidir si un tema está listo, es deuda.

**"completado" como MEDIDA DE CONOCIMIENTO está DEPRECADO.** Respondía a "¿está hecho?", que es una pregunta mal planteada: **¿hecho para quién?** Factorización puede estar terminada para aprobar un semestre y muy lejos de lo que exige un examen de admisión. La doneness real es **derivada y relativa**: un tema está completado *para un objetivo dado* si `nivel_alcanzado >= nivel_requerido` por ese objetivo. Nunca en absoluto.

**Cuidado con la palabra, porque `estado` sí tiene un valor `"completado"` — pero significa otra cosa.** `estado` es puro FLUJO, con tres valores: `no_iniciado` (nunca tocado), `en_progreso` (clase abierta, sin cerrar) y `completado` (**las clases activas sobre el tema terminaron y pasó a la escalera de repaso**). Ese `completado` de flujo NO afirma nada sobre cuánto se sabe: un tema puede estar `estado: "completado"` con `nivel_alcanzado: "base"`. El PASO 5.5 necesita esos tres valores para distinguir "arráncalo" (F) de "retómalo" (A) de "déjalo, ya está en mantenimiento". La profundidad la lleva `nivel_alcanzado`, y solo eso; el flujo lo lleva `estado`, y solo eso.

**Cada objetivo declara qué nivel necesita**, por curso y con override por tema:

```json
"cursos_requeridos": [
  { "curso": "09-mate-superior", "nivel_requerido": "intermedio" },
  { "curso": "02-algebra",       "nivel_requerido": "base",
    "overrides": { "05-factorizacion": "intermedio", "06-fracciones-algebraicas": "intermedio" } }
]
```

El nivel por curso es el default; el override es para los temas de los que cuelga media carrera. **No declares los 88 temas uno a uno**: sería ilegible y se pudriría en dos semanas. Pero tampoco te conformes con el default cuando un tema concreto es un cuello de botella real.

**Para saber si un objetivo está listo, se RECORRE su temario y se comparan niveles**, tema a tema, contra lo requerido. Cuesta un poco más que mirar un booleano. Es lo correcto: un booleano no puede responder "¿listo para qué?".

### La durabilidad: subir la escalera es una inversión, no un gasto

`nivel_repaso` no es burocracia del repaso: **es la predicción de cuánto tiempo el tema sigue vivo.**

| `nivel_repaso` | Retención aproximada |
|---|---|
| 1 | ~1 día |
| 2 | ~3 días |
| 3 | ~7 días |
| 4 | ~14 días |
| 5 | ~30 días |

De ahí sale la regla que lo cambia todo: **un tema está vivo en la fecha de un hito si su último repaso cae dentro de `retención(nivel_repaso)` días antes de esa fecha.**

Y la consecuencia: **cuanto más alto el `nivel_repaso`, menos toques necesita para seguir vivo.** Subir la escalera cuesta repasos ahora y ahorra repasos después. Por eso:

- **Hito lejano** (examen de admisión a 7 meses) → hay que **subir la escalera hasta 5**. Si te quedas en 3, necesitarías un toque cada 7 días durante 30 semanas: insostenible.
- **Hito cercano** (un parcial a 10 semanas) → **la escalera se trunca sola**. Un tema cerrado en la semana 6 solo tiene sitio para 2-3 repasos antes del examen, y con eso llega vivo. No hace falta forzar el 5.

**AVISO OBLIGATORIO — predicción de olvido.** Al planificar un hito, calcula con qué `nivel_repaso` va a llegar cada tema y **avisa de los que llegarán fríos**: *"factorización llega al parcial en nivel 3 (retiene ~7 días) y el examen está a 30 días de su último repaso: va a estar olvidada."* Este aviso es lo único que distingue un plan que cuenta horas de un plan que predice resultados. **No lo omitas por no dar malas noticias.**

Nunca trates igual un tema que completó la escalera y uno que se cortó en el nivel 3 por falta de tiempo. **No están igual de aprendidos, y el plan no puede fingir que sí.**

### El mantenimiento CUESTA HORAS y hay que contarlas

> **Nota de cambio (2026-07-16):** hasta esta fecha las validaciones de deadline contaban **solo el coste de aprender** los temas e ignoraban por completo el de mantenerlos. Con 115 temas en 74 días, la escalera dispara **428 repasos** — unas 70-85 h que el plan no veía, contra un margen reportado de 18 h. **El plan decía "cabe" y no cabía.** Los métodos estaban documentados y aplicados, pero no presupuestados.

Al calcular la viabilidad de un hito, suma **las dos partidas**:

```
horas_aprender    = Σ (saltos de nivel necesarios) × horas_por_sesion × 1.2
horas_mantener    = (nº de repasos que la escalera dispara entre el cierre
                     de cada tema y la fecha del hito) × minutos_por_repaso
horas_necesarias  = horas_aprender + horas_mantener
```

**El 70/30 del Método 3 y la escalera son el MISMO fenómeno, no dos.** El 30% de cada clase es exactamente donde caen esos repasos intercalados. **No los sumes dos veces**: cuenta los repasos que la escalera dispara y ya está.

`minutos_por_repaso` se mide desde `historial.json` igual que `horas_por_sesion`. Mientras no haya datos, usa **12 minutos** y **declara que es una suposición**.

> **Nota de cambio (2026-07-16):** aquí vivía la sección "El listón" (`liston` `dominar`/`pasar`, `sesiones_estimadas` / `sesiones_estimadas_pasar`, y un cobro por diferencia medido **en horas**). Se borró: la sustituye "El conocimiento se mide en DOS dimensiones". El listón no era otra cosa que **el `nivel_requerido` que cada objetivo declara** — `pasar` ≈ `base`/`intermedio`, `dominar` ≈ `avanzado` —, y el coste de un tema ya no es un precio fijo por listón sino la suma de los **saltos de nivel** que faltan (`sesiones_por_nivel`). Su idea más valiosa, el **cobro por diferencia**, sobrevive intacta: es el `proyectado[]` y el "cobro por SALTO DE NIVEL" del algoritmo de más abajo — un objetivo que necesita `avanzado` parte del nivel que otro ya dejó y paga solo lo que falta. Si ves `liston`, `sesiones_estimadas` o `ya_cobrados` en un archivo, es deuda.

### Los objetivos compiten en una cola con prioridad

**Los objetivos NO tienen `horas_semanales` propias.** Hay un único presupuesto y compiten por él.

> **Nota de cambio (2026-07-16):** existió un `horas_semanales` por objetivo. Se eliminó. Con cursos compartidos la pregunta "¿esta hora de trigonometría sale del presupuesto del semestre o del de la UNI?" **no tiene respuesta**, porque la hora sirve a los dos. Cualquier reparto ahí es ficción con apariencia de control. Si ves ese campo en un archivo viejo, es deuda.

La viabilidad se calcula como una **cola con prioridad, cobrando cada curso una sola vez**:

```
NIVELES = [null, "base", "intermedio", "avanzado"]      # ordenados
horas_por_sesion    ← medido desde historial.json (ver regla 10)
minutos_por_repaso  ← medido desde historial.json (12 si no hay datos)
horas_disponibles(fecha) ← integral de presupuesto_horas sobre [hoy, fecha]
horas_comprometidas_hasta(fecha) ← 0 para toda fecha

# El ESTADO es la verdad. Arranca de progreso.json, no de un contador.
nivel[tema]  ← progreso.nivel_alcanzado   # profundidad REAL, verificada
proyectado[tema] ← nivel[tema]            # a dónde lo habrán subido los objetivos ya procesados

# Cierre transitivo: un tema arrastra lo que necesita para existir.
# CRITERIO DE PARADA: lo YA SUFICIENTE para ESTE objetivo, no un
# "completado" absoluto — que es la pregunta mal planteada que el
# rediseño elimina. Un tema en nivel 'base' está "completado" para el
# semestre y sigue faltándole todo para la UNI: depende de quién pregunta.
cerradura(temas, objetivo):
    pendientes ← temas
    resultado  ← conjunto vacío
    mientras pendientes:
        t ← saca uno
        si proyectado[t] >= nivel_requerido(t, objetivo):  continúa   # ya suficiente para este objetivo: ni se aprende ni se arrastran sus bases
        si t ya está en resultado:    continúa   # corta ciclos accidentales
        resultado += t
        pendientes += t.prerrequisitos_internos  (del mismo curso)
        pendientes += t.prerrequisitos_externos  ("curso/tema_id")
    devuelve resultado

para cada objetivo ACTIVO, en orden de prioridad (1 primero):
    para cada hito NO condicional del objetivo, por fecha ascendente:
        semilla ← temas de sus cursos_requeridos que pertenecen a ese
                  hito (por hito_id; si el temario no declara hito_id,
                  todos los del curso) con proyectado[T] < nivel_requerido(T, objetivo)
        temas ← cerradura(semilla, objetivo)

        # ---- 1. APRENDER: cobro por SALTO DE NIVEL, no por sesiones gastadas
        sesiones ← 0
        para cada T en temas:
            requerido ← nivel_requerido(T, objetivo)   # curso + override
            actual    ← proyectado[T]
            si actual >= requerido: continúa           # ya lo tiene: gratis
            para cada paso de actual+1 hasta requerido:
                sesiones += T.sesiones_por_nivel[paso]
            proyectado[T] ← requerido
        horas_aprender ← sesiones × horas_por_sesion × 1.2

        # ---- 2. MANTENER: lo que la escalera dispara hasta el hito
        repasos ← 0
        para cada T en temas:
            simula la escalera desde el cierre estimado de T hasta hito.fecha
            repasos += nº de repasos que caben
        horas_mantener ← repasos × minutos_por_repaso / 60

        horas_necesarias ← horas_aprender + horas_mantener
        horas_disp       ← horas_disponibles(hito.fecha)
                           − horas_comprometidas_hasta(hito.fecha)
        ¿CABE? → horas_necesarias <= horas_disp
        horas_comprometidas_hasta(≥ hito.fecha) += horas_necesarias

        # ---- 3. PREDICCIÓN DE OLVIDO (obligatoria, ver "La durabilidad")
        para cada T en temas:
            si retención(nivel_repaso proyectado de T) < días desde su último
               repaso hasta hito.fecha:
                AVISA: T llegará frío a este hito
```

**`proyectado` no es un libro de cuentas: es "a qué nivel lo habrán dejado los objetivos más urgentes".** Si el semestre sube factorización a `base`, la UNI parte de `base` y paga solo los saltos hasta `avanzado`. Es la misma idea del cobro por diferencia, pero sobre un estado verificable en vez de sobre horas gastadas.

**La cerradura es lo que hace honesto el cálculo.** `cursos_requeridos` lista los cursos de los que el objetivo *trata*; todo lo demás entra solo si algún tema lo pide. Un objetivo de aprobar un semestre requiere sus cinco cursos universitarios — y de la base arrastra únicamente los temas que sus prerrequisitos nombran, no los cursos base completos. Requerir cursos enteros donde bastan tres temas es lo que hace que un plan viable se vea imposible.

Léelo así: **el objetivo más urgente reserva primero, y el siguiente hereda lo que sobra.** Un curso compartido lo paga quien lo necesita antes; los demás se lo encuentran hecho y gratis. Eso modela la competencia real en vez de fingir que los objetivos están separados, y elimina el doble conteo de horas sin trucos.

Los hitos con `condicional: true` (sustitutorios, aplazados) **no entran en la planificación normal**: solo ocurren si algo sale mal. No reserves horas para ellos.

### Ciclo de vida: `estado`

Cada objetivo tiene `estado`:

- **`activo`** — compite por horas ahora. Es el único estado que el PASO 5.5 mira.
- **`latente`** — existe, con su plan hecho, pero aún no compite. Despierta cuando se cumple su `condicion_activacion` (aritmética verificable, igual que las condiciones de los cursos `hito`). El PASO 2a la evalúa en cada arranque; el comando `estado` alerta si se cumplió y sigue dormido.
- **`completado`** — terminado. Deja de competir. Sus cursos, glosario e historia **se conservan**: son evidencia del conocimiento.

**Las prioridades no se reordenan con el tiempo: la máquina de estados lo hace sola.** Cuando el objetivo de prioridad 1 se completa, el de prioridad 2 queda automáticamente arriba sin tocar ningún número. No "asciendas" objetivos a mano.

Y por eso un objetivo paralelo **nunca** se mete como curso dentro de otro "para simplificar": tienen ciclos de vida distintos, así que al cerrar el anfitrión el invitado se archivaría sin haber terminado, o impediría cerrar al anfitrión para siempre.

### Hitos: un objetivo puede tener varias fechas de corte

`fecha_limite` sirve cuando un objetivo tiene **una** fecha (un examen de admisión). Pero un semestre universitario no funciona así, y forzarlo a una sola fecha destruye la información que importa.

Cada objetivo declara un array `hitos`:

```json
"hitos": [
  { "id": "parcial", "nombre": "Evaluación Parcial", "fecha": "2026-09-28",
    "cubre": "Primera mitad de cada curso (semanas 1-8)", "condicional": false },
  { "id": "final", "nombre": "Evaluación Final", "fecha": "2026-11-23",
    "cubre": "Segunda mitad (semanas 8-16). NO acumulativo.", "condicional": false },
  { "id": "sustitutorio", "nombre": "Sustitutorio", "fecha": null,
    "cubre": "TODO el semestre. Reemplaza una nota jalada.", "condicional": true }
]
```

**Lo importante no es que haya varias fechas: es que cada una cubre contenido DISTINTO.** Un final que no es acumulativo significa que la segunda mitad del temario tiene una fecha límite propia y la primera ya no. Por eso los temas del `temario.json` pueden declarar **`hito_id`**: dice a qué corte pertenece cada tema. Con eso la pregunta deja de ser "¿alcanzo para el semestre?" —que no significa nada— y pasa a ser "¿tengo lista la primera mitad de Estática para el 28 de septiembre?", que es la que te aprueba o te jala.

Reglas:
- **`condicional: true`** = solo ocurre si algo sale mal (sustitutorios, aplazados). **No reserves horas para ellos** en la planificación normal; su existencia es información de contingencia, no de plan.
- Si el temario de un curso no declara `hito_id` en sus temas, todos sus temas cuentan para el primer hito no condicional del objetivo.
- Un objetivo con una sola fecha necesita un solo hito. No inventes cortes que no existen.
- Si un hito tiene `fecha: null` (aún no se sabe), no lo uses para validar; pídesela al usuario cuando haga falta.

### La escalera de repaso

**En cuanto un tema alcanza por primera vez un `nivel_alcanzado` no nulo** (llegó al menos a `base`), se le arma un repaso futuro en `progreso.json` con dos campos. Se ata a la PROFUNDIDAD alcanzada, no a `estado`: un tema en `base` ya empieza a olvidarse y necesita mantenimiento, esté `en_progreso` o `completado`. (Antes esto se disparaba "cuando el tema pasa a `completado`"; ese estado dejó de ser una medida de conocimiento — ver "El conocimiento se mide en DOS dimensiones".)

- **`nivel_repaso`** — entero 1-5, índice en la escalera de intervalos.
- **`fecha_proximo_repaso`** — `YYYY-MM-DD`, cuándo vuelve a entrar.

| `nivel_repaso` | Intervalo hasta el próximo repaso |
|---|---|
| 1 | 1 día |
| 2 | 3 días |
| 3 | 7 días |
| 4 | 14 días |
| 5 | 30 días |

Cómo se mueve: al armar la escalera por primera vez entra en `nivel_repaso: 1`. Cada vez que el usuario **resuelve bien** sus ejercicios de repaso, sube un nivel y se re-arma con el intervalo nuevo. Cada vez que **falla**, baja a `nivel_repaso: 1` y se re-arma para el día siguiente.

**La escalera es ADAPTATIVA al hito, no fija.** Sube solo hasta donde hace falta para llegar vivo, y ni un peldaño más — cada peldaño de más es un repaso que le robas a otro tema. El `nivel_repaso` OBJETIVO de un tema es **el menor nivel cuya retención cubre la distancia desde hoy hasta el hito no condicional más cercano que incluye ese tema** (tope 5). De ahí:

- **Hito lejano** (un examen de admisión a 7 meses): ni el nivel 5 (retiene ~30 días) cubre la distancia de un solo toque, así que el objetivo es **5** y el tema se re-arma cada 30 días indefinidamente. Al llegar a 5 **no se apaga**: los temas cerrados en julio de 2026 tienen que seguir vivos en febrero de 2027.
- **Hito cercano** (un parcial a 4 semanas): un `nivel_repaso: 3` (retiene ~7 días) puede bastar para cruzar la meta. Alcanzado el objetivo, el tema **se mantiene ahí** (se re-arma al mismo intervalo) en vez de seguir escalando hacia el 5 — forzar el 5 gastaría repasos que ese hito no necesita. La escalera **se trunca sola** además por otro lado: un tema cerrado en la semana 6 solo tiene sitio físico para 2-3 repasos antes del examen.

Cuando un tema sirve a varios hitos (uno cercano y otro lejano), manda el **más lejano** para fijar el objetivo: hay que mantenerlo vivo hasta el último, no solo hasta el primero.

Los intervalos se miden en **días de calendario, no en sesiones**. La curva del olvido es tiempo real: si el usuario estudia tres veces un martes, eso no adelanta ningún repaso.

### Varias sesiones el mismo día

El PASO 5.5 se recalcula desde los archivos en cada sesión, así que este caso se resuelve solo y **no necesita ninguna regla especial**:

- Mañana: clase de aptitud → su `ultima_sesion` queda en hoy → tarde: la rama (C) ya no dispara → la tarde cae en `tema-nuevo`. Protocolo normal.
- Mañana: tema nuevo que quedó a medias → tarde: la rama (A) ve `en_progreso` y lo continúa.
- Mañana: tema nuevo cerrado → tarde: no hay nada en progreso, la cadencia está fresca → siguiente tema nuevo, mencionando la nota de la regla 3 del PASO 5.5.

---

## Qué es este proyecto

Un plan de estudios personalizable. Toda la información se rastrea en archivos JSON dentro de cada carpeta de curso. Tu trabajo como agente es leer esos archivos para adaptar cada sesión al nivel real del usuario y actualizarlos al terminar.

El estado del usuario vive en archivos, no en tu memoria de conversación. Esto permite que:
- El usuario cambie de agente (Claude → GPT → agente local) sin perder progreso.
- Una nueva sesión con el mismo agente arranque desde donde quedó la anterior.
- El plan crezca en el tiempo (ampliaciones de objetivo, ver `_protocolos/expansion.md`).

---

## Protocolo al INICIAR una sesión o clase

Antes de explicar cualquier contenido, lee los siguientes archivos del curso a trabajar:

1. `README.md` del curso — objetivos, prerrequisitos externos.
2. `temario.json` — el árbol de temas del curso, con dependencias internas, objetivos y criterios de dominio.
3. `progreso.json` — qué temas están completados, en progreso o no iniciados (referencia por `tema_id` al temario).
4. `dificultades.json` — dificultades con `"resuelto": false` relevantes al tema de hoy o a sus bases.
5. `glosario.json` — vocabulario que el usuario ya maneja (no re-explicar esos términos desde cero).
6. Si el tema declara `prerrequisitos_externos`, lee también los archivos de esos cursos — pero **solo lo relativo a los temas nombrados**, no el curso entero. Y comprueba en su `progreso.json` que estén `completado`: si un prerrequisito externo no lo está, el tema de hoy probablemente no es el que toca. Dilo antes de arrancar.
7. `objetivo.json` de la raíz — recuerda hacia dónde va el usuario y qué hitos tiene cerca. **Fíjate a qué objetivo(s) sirve el curso de hoy** (en qué `cursos_requeridos` aparece). Si sirve a varios, la `motivacion` y el hito que aplican son los del objetivo que le da su **urgencia efectiva** — y merece la pena decírselo al usuario, porque saber que trigonometría le cae en el parcial de septiembre *y además* en el examen UNI motiva más que cualquiera de las dos por separado. Lo que no vale es motivar una clase de inglés con "esto te sirve para entrar a la UNI": eso es sencillamente falso.

Con esa información adapta la sesión:
- Refuerza las dificultades pendientes relevantes antes o durante la explicación.
- Parte del nivel real que muestra `progreso.json`, no de cero.
- No redefinas términos que ya están en el glosario del usuario con `nivel_dominio: "solido"`.
- Verifica retrasos: si `sesiones_dedicadas` del tema actual supera en >50% la suma de sus `sesiones_por_nivel` hasta el nivel que le pide su objetivo más urgente, menciónalo y ofrece re-planificar. Pasarse de presupuesto NO baja `nivel_alcanzado` — solo dice que le está costando más de lo estimado.
- **Arma la lista de temas a intercalar**: los temas con escalera armada (`nivel_repaso` no nulo) y `fecha_proximo_repaso <= hoy`, ordenados del más vencido al menos vencido. Toma los 2-3 primeros — son los que alimentan el 30% de repaso del bloque de ejercicios (ver Método 3). Si para intercalar bien necesitas ver qué se hizo en esa clase, abre su `clases/NN-tema/README.md`: el coste de contexto es aceptable y los ejercicios salen mucho mejores.

---

## Protocolo al CERRAR o FINALIZAR una clase

Cuando el usuario indique que terminó, actualiza estos archivos:

### `progreso.json`

**Del tema trabajado hoy — lo primero es DECIDIR EL NIVEL, no marcar "hecho":**

`nivel_alcanzado` es el campo central del modelo y **este protocolo es su ÚNICO escritor.** Si esto no corre, el rediseño entero es decorativo. Actualízalo así, en orden:

1. **Averigua el nivel al que la clase de hoy apuntaba** (`nivel_objetivo_hoy`): recorre los objetivos ACTIVOS que requieren este curso y toma el MAYOR `nivel_requerido` para este tema (el default del curso en su `cursos_requeridos`, salvo que un `override` de tema lo suba). Ese es el techo que tiene sentido verificar hoy; no hace falta comprobar `avanzado` si ningún objetivo activo lo pide aún.

2. **Lee el `criterio_dominio` del tema en su `temario.json`.**
   - Si ya es un objeto por nivel (`base`/`intermedio`/`avanzado`), usa el criterio del nivel que estás evaluando.
   - Si todavía es un solo string (herencia del modelo viejo), trátalo como el criterio del nivel para el que se escribió el curso. Y **si vas a verificar un nivel que no tiene criterio específico escrito, escríbelo AHORA** derivándolo de la `rubrica_niveles` del curso (ver "Reglas de calidad del temario"). Este es el único momento en que ese criterio se necesita —cuando decides si el nivel sube— y ni un minuto antes. Guárdalo en el `temario.json` convirtiendo el campo a objeto por nivel.

3. **Verifica contra la evidencia REAL de la clase** (los ejercicios que resolvió + el Feynman de cierre), subiendo desde `nivel_alcanzado` actual **+1** hacia `nivel_objetivo_hoy`, **un nivel a la vez**:
   - Si el usuario cumplió el criterio de ese nivel → sube `nivel_alcanzado` a ese nivel y evalúa el siguiente.
   - En el PRIMER nivel cuyo criterio NO se cumplió → **detente ahí.** `nivel_alcanzado` se queda en el último verificado.
   - **`nivel_alcanzado` solo sube con evidencia verificada, nunca por haber gastado la sesión.** Si el usuario estudió pero sigue fallando el criterio de `base`, `nivel_alcanzado` se queda como estaba (posiblemente `null`) y `estado` queda `en_progreso`. El plan tiene que poder decir "se sentó a estudiarlo y todavía no le sale" — un contador de esfuerzo diría "pagado, siguiente" y mentiría.
   - **Nunca BAJES `nivel_alcanzado` al cerrar una clase de tema nuevo.** La profundidad no se pierde por una clase floja; el olvido lo modela `nivel_repaso`, no esto.

4. **`estado` (flujo, NO conocimiento):**
   - `no_iniciado` → `en_progreso` al arrancar la clase.
   - `en_progreso` → `completado` **solo** si la clase se cerró de verdad y `nivel_alcanzado` llegó al `nivel_objetivo_hoy` (o el usuario decide dar por terminado el trabajo activo sobre el tema y pasarlo a mantenimiento). Aquí `completado` significa **"las clases activas sobre este tema terminaron y pasó a la escalera de repaso"** — NO es una medida de conocimiento (esa es `nivel_alcanzado`). Un tema puede quedar `completado` en `nivel_alcanzado: base`.
   - **Si la clase se cortó a medias** (el usuario paró antes de que se pudiera verificar el criterio, o el handoff se interrumpió): `estado` se queda en `en_progreso` y `nivel_alcanzado` **NO sube**. Una clase que no llegó a verificar nada no cambia la profundidad. La rama (A) del PASO 5.5 la retomará.

5. **`ultima_sesion`**: fecha y hora en ISO 8601 (`YYYY-MM-DDTHH:MM`). La hora es informativa; los repasos se cuentan por día de calendario.

6. **`sesiones_dedicadas`**: +1.

7. **Arma la escalera de repaso cuando el tema alcanza por primera vez un `nivel_alcanzado` no nulo** (llegó al menos a `base`): pon `nivel_repaso: 1` y `fecha_proximo_repaso` = mañana. Ver "La escalera de repaso". **La escalera se ata a la PROFUNDIDAD alcanzada, no a `estado`:** un tema que llegó a `base` ya empieza a olvidarse y necesita mantenimiento, esté `en_progreso` o `completado`. Si el tema ya tenía escalera armada de una sesión previa, no la reinicies aquí.

**De los temas que se intercalaron en el bloque de ejercicios** (no del tema nuevo):
- Si el usuario los resolvió bien: sube `nivel_repaso` en 1 (tope 5) y recalcula `fecha_proximo_repaso` = hoy + el intervalo del nivel nuevo.
- Si falló: baja `nivel_repaso` a 1 y pon `fecha_proximo_repaso` = mañana. Además, registra la dificultad en `dificultades.json` como cualquier otra.
- No toques su `estado`, `nivel_alcanzado` ni `sesiones_dedicadas`: un intercalado no es una sesión del tema viejo, y resolver bien un repaso confirma durabilidad (`nivel_repaso`), no más profundidad. Sí actualiza su `ultima_sesion`.
- Si un tema intercalado resultó estar realmente roto (falló varios ejercicios, no solo uno), dilo al cerrar y sugiere que la próxima sesión sea una clase de `repaso`. La rama (B) del PASO 5.5 probablemente ya lo dispare sola, pero avisar es mejor que sorprender.

### `historial.json` (raíz) — APPEND, nunca overwrite

**Pregúntale al usuario cuánto duró la sesión.** No lo estimes: es el único dato que este archivo existe para capturar, y estimarlo lo convierte en basura con apariencia de medición. Una pregunta corta al cerrar ("¿cuánto duró, más o menos?") basta.

Agrega **una entrada al final** del array `sesiones`:

```json
{
  "inicio": "2026-07-16T09:30",
  "duracion_min": 95,
  "tipo_clase": "tema-nuevo",
  "objetivo_id": "obj-1",
  "curso": "01-aritmetica",
  "tema_principal": "09-porcentajes",
  "temas_intercalados": ["04-divisibilidad", "06-fracciones"],
  "nota": null
}
```

- **Nunca modifiques ni borres entradas anteriores.** Es un registro histórico: su valor está en que no se toca.
- Una entrada por sesión, aunque la sesión haya tocado varios temas (los intercalados van en su campo).
- Si la sesión vino de un handoff externo, ponlo en `nota` y usa la duración que reportó el resumen.

### `dificultades.json`
- Agrega una entrada por cada concepto que el usuario no entendió bien o preguntó repetidamente. Usa la estructura completa: `id`, `curso`, `tema_id`, `tipo` (conceptual/operativa/notacion/aplicacion), `descripcion`, `error_tipico`, `fecha_deteccion`, `fecha_ultimo_repaso`, `veces_repasado`, `resuelto`, `estrategia_que_ayuda`.
- Si una dificultad existente fue trabajada en la clase, actualiza `fecha_ultimo_repaso` e incrementa `veces_repasado`.
- Si la dificultad se resolvió, marca `resuelto: true` y llena `estrategia_que_ayuda` con qué técnica funcionó.

### `glosario.json`
- Agrega términos nuevos con estructura completa: `termino`, `definicion` (en palabras del usuario), `curso`, `tema_id`, `ejemplo`, `terminos_relacionados`, `nivel_dominio` (`"reconoce"` / `"solido"` / `"vacilante"`), `fecha_aprendido`, `fecha_ultimo_uso`.
- Si un término ya existe y volvió a aparecer, actualiza `fecha_ultimo_uso` y ajusta `nivel_dominio` si cambió.
- No dupliques entradas.

### `clases/NN-tema/README.md`
- Si no existe la carpeta de la clase, créala copiando `_plantillas/plantilla-README-clase.md`.
- Completa apuntes y dudas.
- **Regla obligatoria para las secciones "Ejercicios trabajados" y "Feynman de cierre":** no resumas ni parafrasees. Cita el enunciado completo (o la consulta exacta del agente, en el caso de Feynman) y la respuesta del usuario copiada de forma **textual/verbatim** — tal cual la escribió, incluso con errores de tipeo. Después de cada cita, agrega una **anotación propia del agente** (qué reveló esa respuesta: método usado, acierto, error, duda, ideas para sesiones futuras). Esto preserva la voz real del usuario para diagnóstico futuro en vez de una versión reinterpretada por el agente. Ver el formato exacto en `_plantillas/plantilla-README-clase.md`.

---

## Reglas de calidad del temario

> Esta sección es obligatoria cuando generes o modifiques un `temario.json`. Un mal árbol de temas hunde todo el curso.

1. **Ingeniería inversa desde el objetivo del curso**: parte del último tema (lo que el usuario debe saber hacer al final) y pregunta recursivamente "¿qué se necesita saber para esto?". No listar temas al azar ni copiar índices literales de libros.
2. **Atomicidad**: cada tema debe caber en 1-4 sesiones. Si estima más, pártelo.
3. **Un solo salto conceptual por tema**: no mezclar "derivadas + regla de la cadena + aplicaciones" en un mismo tema.
4. **Prerrequisitos explícitos, internos y externos, SIEMPRE a nivel de tema**: cada tema declara de qué otros temas depende — del mismo curso en `prerrequisitos_internos` (por `id`), y de otros cursos en `prerrequisitos_externos` (formato `"curso/tema_id"`). No puede haber ciclos.

    **Nunca declares una dependencia a nivel de curso.** "Estática necesita geometría" es cierto e inútil: lo que necesita es `03-geometria/09-areas` para momentos de inercia, no las cónicas ni los sólidos de revolución. Una dependencia a nivel de curso arrastra el curso entero a la planificación y **infla el plan con material que nadie va a preguntar** — el plan se ve imposible cuando no lo es, que es otra forma de mentir. El README del curso puede llevar un resumen en prosa para humanos, pero la verdad operativa vive en el temario.

    Y si necesitas expresar una dependencia y no tienes dónde, **no la metas en `referencias` ni en la descripción**. Ese campo existe; úsalo.
5. **Objetivos observables**: cada tema tiene 2-4 objetivos que empiezan con **verbo de acción** (calcular, demostrar, aplicar, identificar, resolver, traducir). Prohibido "entender X" — no es verificable.
6. **Rúbrica de curso + criterio de dominio por nivel.** Todo `temario.json` declara, a nivel de curso, una **`rubrica_niveles`** que define qué significan `base`, `intermedio` y `avanzado` **en ese curso**. Regla de diseño irrompible (decidida por el usuario, 2026-07-16): los niveles se expresan **como CAPACIDADES, nunca como el puntaje en el examen de un tercero.** Anclar la vara a "8/10 tipo UNI" ata la medición a una entidad externa que solo está calibrada para algunos cursos (sirve para matemática, no para razonamiento verbal ni para un idioma). Escribe: `base` = resuelve lo directo del tema y reconoce cuándo aplica; `intermedio` = resuelve ejercicios variados sin ayuda y los combina; `avanzado` = reconoce el tipo de problema al leerlo, resuelve bajo presión de tiempo, aguanta los casos raros. Adáptalo a cada curso.

    El **`criterio_dominio` específico por tema y nivel** se DERIVA de esa rúbrica, pero **NO se fabrica por adelantado**: se escribe cuando el tema se estudia —que es justo cuando se necesita para decidir si `nivel_alcanzado` sube (lo hace el "Protocolo al CERRAR una clase")— y ni un minuto antes, para no llenar el temario de criterios que se pudren sin usarse. Mientras un tema no se haya estudiado bajo el modelo nuevo, su `criterio_dominio` puede ser un solo string (el del nivel para el que se escribió el curso); trátalo así hasta convertirlo en objeto por nivel el día que se estudie. Escribe criterios observables (ej: "resuelve 8/10 ejercicios variados sin ayuda en menos de X min"), nunca "entiende el tema".
7. **Validación con bibliografía**: los temas cubren los capítulos relevantes de la bibliografía acordada. La bibliografía es **obligatoria** y se guarda dentro de `temario.json`.
8. **Dificultad progresiva pero no lineal**: `dificultad: 1-5`. Alterna para permitir intercalado en ejercicios.
9. **Marca temas opcionales**: `opcional: true` para temas que enriquecen pero no son ruta crítica. Sirven para recortar si aprieta el tiempo.
10. **Ajuste al deadline — cola con prioridad, por hito**: aplica el algoritmo de "El presupuesto: una cola con prioridad, no un reparto" (sección "Objetivos paralelos"). Recorre los objetivos activos por prioridad, y dentro de cada uno sus hitos no condicionales por fecha; cobra cada tema **una sola vez** y descuenta las horas ya comprometidas por los objetivos más urgentes. Si algún hito no cabe, reporta y propón opciones.

    **`HORAS_POR_SESION` se MIDE, no se supone.** Calcúlalo desde `historial.json`: `promedio(duracion_min) / 60` sobre las sesiones registradas.
    - **Con ≥5 sesiones en el historial** → usa el promedio medido y dilo: *"asumiendo tus 1.8 h/sesión reales (medido sobre 23 sesiones)"*.
    - **Con <5 sesiones** → usa 1.5 h como provisional y **declara que es una suposición**: *"asumiendo sesiones de 1.5 h, que aún no está medido"*. Nunca presentes el resultado como un hecho.

    **`presupuesto_horas` también son promesas, no mediciones.** Es lo que el usuario *dijo* que puede dedicar en cada periodo. Si el historial tiene ≥3 semanas de datos, compara contra las horas reales por semana **del periodo que corresponda** y, si difieren en más del 30%, dilo antes de dar por buena cualquier proyección. Un plan validado contra horas que no ocurren es un plan que miente con aritmética correcta.

    **Nunca cobres dos veces un curso que comparten dos objetivos.** Estudiar trigonometría una vez sirve a los dos pero consume horas una vez; contarla en ambos infla el plan. El `proyectado[]` del algoritmo existe exactamente para eso: el objetivo más urgente sube el nivel y paga los saltos, y el siguiente parte de ese nivel y solo paga la diferencia que le falte.

    Si un objetivo no tiene hitos con fecha (todo `null`), sáltate esta validación para él — pero sus cursos siguen compitiendo por las horas de los demás, así que sus temas sí actualizan `proyectado[]` cuando les toque.
11. **`modo_estudio` obligatorio**: todo `temario.json` declara cómo se estudia el curso (`ruta`, `cadencia` o `hito`). Sin este campo el PASO 5.5 no puede colocar el curso en ninguna clase y el curso queda huérfano — que es exactamente el bug que tuvo este proyecto hasta julio de 2026. Ver "Cómo se estudia cada curso" para la estructura y los valores.
12. **El grafo crece desde la necesidad hacia atrás, nunca por completitud.** Cuando un curso nuevo necesite algo que no existe todavía:
    - **¿El tema falta en un curso que ya existe?** → agrégalo a ESE curso. No crees un curso paralelo ni una versión "para el otro objetivo".
    - **¿El curso entero no existe?** → créalo, pero **solo con los temas que hacen falta**, no con los treinta que "debería tener" un curso de esa materia. Un curso de trigonometría creado para dar soporte a Estática puede tener tres temas y estar perfecto.
    - **Y hazlo transitivamente**: si el tema que agregas depende de otro que tampoco tienes, ese también entra, y así hacia atrás hasta tocar algo que el usuario ya tiene a nivel suficiente (`nivel_alcanzado` que cubre lo que el tema nuevo necesita) o que sea base real.
    - **Ese es el criterio de parada**: se para cuando llegas a algo ya sabido, no cuando el curso "se ve completo". Añadir temas que nadie requiere es exactamente lo que hace que un plan no quepa.

---

## Re-planificación (dentro de un objetivo ya activo)

**Bajo demanda**: si el usuario pregunta "¿voy bien?", "¿alcanzo con el deadline?", etc., corre la cola con prioridad por hito (ver "Los objetivos compiten en una cola con prioridad") y compara `nivel_alcanzado` real tema a tema contra `nivel_requerido`, sumando las horas de los saltos que faltan + el mantenimiento. **Hazlo por objetivo, no en bloque** — si tiene dos objetivos activos, puede ir sobrado en uno y hundido en el otro, y un promedio te ocultaría las dos cosas.

**Automática (solo si hay atraso grande)**: al inicio de una sesión, si detectas atraso >50% respecto al plan (temas completados vs. esperados según fecha), avisa proactivamente antes de arrancar contenido.

**Nunca menciones el deadline en cada sesión** si el usuario va bien. Sé discreto: molesta y desmotiva.

Opciones a proponer cuando hay desvío:
- Marcar temas `opcional: true` como omitidos.
- Subir las horas de algún periodo de `presupuesto_horas` — solo si el usuario confirma que existen de verdad. No las inventes tú. Y fíjate en **qué periodo**: subir las de un tramo que ya pasó no sirve de nada.
- **Bajar el `nivel_requerido` de un objetivo** en un curso (o quitar un `override`): de `avanzado` a `intermedio`, de `intermedio` a `base`. Es la palanca más potente y la más malentendida: **no elimina trabajo, lo aplaza** — el objetivo que sí necesita el nivel alto pagará después los saltos que este dejó sin subir (el `proyectado[]` lo hace explícito). Cuando la propongas, di siempre cuánto le costará después a ese otro objetivo.
- Mover la fecha de un hito, si el hito es negociable (uno de examen universitario no lo es; uno autoimpuesto sí).
- **Ceder desde el objetivo menos prioritario**: relajar la `cadencia_dias` de sus cursos (de cada 2 días a cada 4, por ejemplo) para que libere horas. Con el presupuesto en cola, es **la** palanca de reparto: no hay un número de horas que mover, así que se afloja el ritmo del que puede esperar. Propónsela al usuario, nunca la apliques por tu cuenta.

**Cuando el objetivo prioritario va atrasado, lo que se recorta es el menos prioritario, no al revés.** Si el de `prioridad: 1` está hundido y el de `prioridad: 3` va al día, la propuesta correcta es aflojar el 3 temporalmente. Que el usuario decida — pero preséntale esa opción primero, no la de mover el hito del que importa.

**Ojo con los cursos compartidos al recortar.** Antes de marcar un tema como omitido, comprueba si su curso lo requiere **otro** objetivo además del que estás recortando. Recortar trigonometría "porque la UNI puede esperar" también le quita la base a Física III del semestre. Dilo si pasa.

**Diferencia con ampliación:** re-planificación es ajustar un objetivo ya activo. Agregar un objetivo nuevo (aunque sea en paralelo) se maneja en `_protocolos/expansion.md`.

---

## Herramientas y formato de ejercicios

Un agente de texto tiene límites obvios cuando el estudio requiere gráficos, notación especial o entornos de ejecución. El proyecto asume un modelo mixto: el agente guía y valida, pero se apoya en herramientas externas y en el "instrumento físico" del usuario (cuaderno, VM, instrumento musical, etc.).

### Cómo debe proceder el agente al proponer un ejercicio

Cada `temario.json` puede declarar un campo `herramientas_recomendadas` a nivel de curso (opcionalmente restringidas a temas concretos vía `para_temas`). Al proponer un ejercicio, el agente debe:

1. **Indicar la herramienta a usar** para resolverlo (cuaderno, Desmos, WolframAlpha, VM con Docker, LilyPond, etc.).
2. **Especificar el formato en que espera la respuesta** del usuario: texto plano, LaTeX, código, foto de cuaderno (descrita verbalmente), salida de una herramienta externa.
3. **Ofrecer una vía de verificación**: si el usuario duda de su resultado, sugerir la herramienta apropiada (WolframAlpha, Symbolab, ejecutar un test, etc.).
4. **No forzar la digitalización**: si el usuario está resolviendo a mano, aceptar transcripción parcial ("me trabé en el paso X, así llegué hasta aquí") en lugar de exigir la solución completa transcrita.

### Guía detallada de herramientas

La descripción completa de cada herramienta (qué es, cómo se usa, ejemplos concretos, alternativas) vive en [`_recursos/herramientas.md`](_recursos/herramientas.md). El agente debe consultarla cuando el usuario pregunte por una herramienta que no conoce, y actualizarla si detecta que la información quedó desactualizada (protocolo dentro de ese archivo).

### Panorama por área

Estas son sugerencias orientativas. Ver detalle en `_recursos/herramientas.md`. Durante el setup el agente debe adaptar a la disciplina real del usuario.

**Matemáticas (aritmética, álgebra, cálculo, etc.)**
- Notación: LaTeX en los apuntes (`$x^2 + 2x$` renderiza en editores con soporte Markdown+Math como VS Code o Obsidian).
- Verificación: WolframAlpha, Symbolab.
- Gráficos: Desmos, GeoGebra.
- Instrumento del usuario: cuaderno físico para el desarrollo paso a paso.

**Geometría / Física con diagramas**
- Diagramas simples: ASCII art o Mermaid inline en el README de la clase.
- Complejos: el agente puede generar código GeoGebra/Desmos/SVG y guardarlo en la carpeta de la clase; el usuario lo abre en el navegador.
- Simulaciones físicas: PhET (phet.colorado.edu).

**Programación**
- Claude Code brilla aquí: puede crear archivos, ejecutar código, correr tests. Prácticamente sin fricción.
- Otros agentes sin ejecución: el usuario ejecuta en su entorno y pega output.

**Ciberseguridad**
- Labs reales: TryHackMe, HackTheBox, VulnHub.
- Entornos locales: Docker/VM. Claude Code puede montar Dockerfiles.
- El agente actúa como "compañero de laboratorio" que guía la enumeración/explotación paso a paso.

**Música (teoría)**
- Partituras: LilyPond (código → PDF) o MuseScore.
- Diagramas de escalas/círculo de quintas: ASCII/SVG.
- Audio: el agente describe intervalos, el usuario los toca/escucha en su instrumento.

**Idiomas**
- Texto puro funciona para gramática y vocabulario.
- Pronunciación: Forvo, apps de conversación externas.
- Repetición espaciada de vocabulario: el `glosario.json` con `fecha_ultimo_uso` cubre esto naturalmente.

### Regla general

**Si el agente no tiene la capacidad para verificar directamente algo (gráfico, audio, código en ejecución), debe decírselo al usuario y proponer cómo verificarlo juntos.** No fingir que "todo se puede resolver en el chat" — es mejor delegar la verificación a una herramienta apropiada.

---

## Métodos de estudio que el agente debe aplicar

Este proyecto usa un conjunto específico de técnicas pedagógicas con respaldo científico. A continuación se describe cada una con precisión para que el agente las aplique correctamente y no las confunda con instrucciones genéricas de comportamiento.

---

### Método 1: Práctica de Recuperación Activa
**Qué es:** Técnica donde el estudiante intenta recordar información desde cero, sin tener el material delante, en lugar de releerlo o escucharlo pasivamente. El acto de esforzarse por recuperar un recuerdo —aunque cueste— es lo que consolida el aprendizaje en la memoria a largo plazo. No confundir con "repasar apuntes" o "escuchar una explicación", que son formas pasivas.

**Cómo lo aplica el agente:**
- Al inicio de cada clase, antes de introducir contenido nuevo, pregunta al usuario qué recuerda del tema anterior. No le muestres los apuntes todavía.
- Haz preguntas directas: "¿Puedes explicarme con tus palabras qué es X?", "¿Cuál es el resultado de Y?", "¿Cómo se resuelve este tipo de problema?"
- Espera la respuesta del usuario antes de confirmar o corregir. No adelantes la respuesta.
- Anota mentalmente qué recordó bien y qué no, para enfocarte en los huecos durante la clase.
- **No hagas recuperación activa de algo que se estudió HOY.** Si el `ultima_sesion` del tema anterior es la fecha de hoy (el usuario estudió en la mañana y volvió en la tarde), sáltala o redúcela a una frase. Preguntarle qué recuerda de algo que cerró hace dos horas no mide retención, molesta: la respuesta está en memoria de trabajo, no en memoria a largo plazo, así que el ejercicio no informa nada y se siente redundante. Arranca directo. Lo mismo vale para el intercalado: un tema cerrado hoy no está vencido y no entra (su `fecha_proximo_repaso` es mañana, como mínimo).

---

### Método 2: Repetición Espaciada
**Qué es:** Técnica donde los temas se repasan en intervalos de tiempo crecientes en lugar de estudiarlos todo de una sola vez. La curva del olvido de Ebbinghaus muestra que olvidamos rápido lo recién aprendido, pero si repasamos justo antes de olvidar, el recuerdo se vuelve más duradero y el intervalo antes del siguiente repaso aumenta. No confundir con "repasar seguido" o "repetir muchas veces el mismo día".

**Intervalos recomendados:** 1 día → 3 días → 1 semana → 2 semanas → 1 mes.

**Cómo lo aplica el agente:** en tres niveles distintos, no solo uno. Un tema puede estar "completado" y sin dificultades abiertas y aun así estarse olvidando — por eso el nivel de temas es el más importante y el que no puede faltar.

**Nivel 1 — temas completados (el principal).** Es lo que sostienen los campos `nivel_repaso` y `fecha_proximo_repaso` de `progreso.json`, con la escalera definida en "La escalera de repaso". El PASO 5.5 los lee para decidir la clase, y el "Protocolo al INICIAR" arma con ellos la lista de temas a intercalar. **El repaso de temas se entrega intercalado dentro de la clase del día (Método 3), no como un bloque aparte** — la clase de `repaso` dedicada es solo el escape hatch cuando se acumulan o cuando algo se rompió.

**Nivel 2 — dificultades.** Al leer `dificultades.json` al inicio de la sesión, calcula cuántos días pasaron desde `fecha_ultimo_repaso` de cada dificultad no resuelta. Si tiene entre 1 y 3 días desde la última vez que se trabajó, es candidata a repaso en esta sesión. Incluye un repaso breve de esas dificultades antes de continuar con el tema nuevo. Al cerrar la clase, registra `fecha_ultimo_repaso` para poder calcular el próximo intervalo.

**Nivel 3 — vocabulario.** En `glosario.json`, los términos con `fecha_ultimo_uso` antigua y `nivel_dominio: "reconoce"` son candidatos a repaso rápido.

---

### Método 3: Aprendizaje Intercalado
**Qué es:** Técnica donde, dentro de una misma sesión de estudio, se mezclan ejercicios o preguntas de distintos temas en lugar de agotar uno completamente antes de pasar al siguiente. Esto obliga al cerebro a identificar qué tipo de problema tiene enfrente y qué estrategia aplicar, lo que desarrolla una comprensión más flexible y transferible. No confundir con "estudiar varios temas en la misma semana" ni con cambiar de tema cuando uno se hace difícil.

**Cómo lo aplica el agente:**

- **La mezcla estándar del bloque de ejercicios es ~70% tema nuevo / ~30% temas vencidos.** Los temas vencidos son los que armó el "Protocolo al INICIAR" a partir de `fecha_proximo_repaso` — normalmente 2-3.
- **El intercalado NO es solo para el repaso.** Aunque ningún tema esté vencido, sigue mezclando: temas relacionados, prerrequisitos del tema de hoy, cualquier cosa ya vista que se conecte. El 30% es un piso, no un techo.
- No agotes un tema antes de pasar al siguiente ni sigas un orden predecible: un ejercicio del tema actual, uno de un tema anterior, otro del actual más difícil.
- Al generar series, varía también el *tipo* de problema (numérico, simbólico, verbal, gráfico), no solo el tema. Ver Método 6.
- **Esta regla convive con `una_clase_por_sesion`, no choca con ella.** Intercalar ejercicios de temas ya cerrados no es "cambiar de tema": es parte de la clase de hoy. Lo que la configuración limita es abrir un *segundo tema nuevo*. Ver "Anatomía de una clase".

---

### Método 4: Interrogación Elaborativa
**Qué es:** Técnica donde el estudiante no solo aprende el "qué" sino que construye activamente el "por qué" y el "cómo se conecta con lo que ya sé". Se basa en hacer preguntas que obligan a relacionar conceptos nuevos con conocimiento previo, lo que crea redes de significado más ricas y duraderas. No confundir con simplemente dar más contexto o ejemplos; la clave es que sea el estudiante quien construya la respuesta.

**Cómo lo aplica el agente:**
- Después de explicar un concepto nuevo, no preguntes si lo entendió. En cambio, pregunta: "¿Por qué crees que esto funciona así?", "¿En qué se parece esto a [concepto previo]?", "¿Qué pasaría si [condición cambia]?"
- Cuando el usuario responda, no corrijas de inmediato si hay un error parcial. Primero pregunta: "¿Por qué dices eso?" para que él mismo identifique la falla.
- El objetivo es que el usuario construya la explicación, no que la reciba.

---

### Método 5: Técnica Feynman
**Qué es:** Técnica de comprensión profunda donde el estudiante explica un concepto como si se lo estuviera enseñando a alguien que no sabe nada del tema, usando lenguaje simple y sin jerga técnica. Las lagunas de comprensión se vuelven evidentes inmediatamente cuando no se puede explicar algo con palabras propias. No confundir con hacer un resumen o repetir la definición del libro; la clave es la simplificación real y detectar dónde se rompe la explicación.

**Cómo lo aplica el agente:**
- Al cierre de cada clase, pide al usuario: "Explícame [tema de hoy] como si yo no supiera nada del área."
- Escucha la explicación y detecta: ¿usó términos técnicos sin explicarlos? ¿saltó pasos? ¿hay algo que no pudo explicar?
- Por cada laguna detectada, haz una pregunta puntual sobre ese punto específico, no sobre el tema general.
- Si el usuario logra explicarlo bien, registra el tema como sólido. Si no, marca los puntos flojos en `dificultades.json`.

---

### Método 6: Práctica con Problemas Variados
**Qué es:** Técnica donde el estudiante resuelve muchos problemas distintos del mismo tema en lugar de repetir el mismo tipo de ejercicio. La variedad obliga a identificar qué estrategia aplicar en cada caso, en lugar de ejecutar un procedimiento mecánico. No confundir con "hacer muchos ejercicios"; la clave es la variedad en el tipo de problema, no la cantidad.

**Cómo lo aplica el agente:**
- Al generar ejercicios, nunca des dos del mismo tipo seguidos. Varía: numérico, simbólico, con contexto verbal, con gráfica, de aplicación real.
- Aumenta la dificultad de forma progresiva pero no lineal — alterna fácil y difícil para mantener la atención.
- Si el usuario comete el mismo error en varios ejercicios del mismo tipo, detente y trabaja ese punto específico antes de seguir.

---

### Cuándo aplicar cada método durante la sesión

| Momento de la sesión | Métodos a aplicar |
|---|---|
| Antes de la clase (PASO 5.5) | Repetición Espaciada — nivel de temas: decide qué clase toca y qué temas van vencidos |
| Inicio de clase | Práctica de Recuperación Activa + Repetición Espaciada — niveles de dificultades y vocabulario |
| Durante la explicación | Interrogación Elaborativa |
| Durante los ejercicios | Práctica con Problemas Variados + Aprendizaje Intercalado (~70/30 con los temas vencidos) |
| Cierre de clase | Técnica Feynman + actualización de archivos JSON (incluida la escalera de repaso) |

---

### Reglas generales de comportamiento

- **Notación matemática (LaTeX): cómo mostrarla bien al usuario**. El usuario quiere ayuda visual con fórmulas (le importa cada vez más de cara a física, química y matemática avanzada). El punto clave, confirmado en la práctica: **escribir LaTeX como texto plano en el chat NO se renderiza en este entorno** (app de escritorio de Claude / Claude Code); el `$...$` o `$$...$$` aparece crudo sin importar que la sintaxis sea correcta — no es un problema del modelo ni de la sintaxis, sino de que el renderer del chat es Markdown sin motor matemático (KaTeX/MathJax). **NUNCA escribas LaTeX suelto directamente en el texto del chat** (ni siquiera "de paso" o como aparte) — es un error, no una opción, sin importar que la fórmula sea trivial o central. Fuera de eso, hay **dos vías que SÍ funcionan, y NO son mutuamente excluyentes** — evalúa cada una por separado, no elijas "una u otra":
    1. **¿Vale la pena conservarla para repaso futuro?** → escríbela en `clases/NN-tema/README.md` con LaTeX normal (`$...$`). El usuario la abre en la vista previa de VS Code/Obsidian y ahí renderiza perfecto. Aplica a toda fórmula/teoría seria del tema (casi siempre sí).
    2. **¿Es el centro de la explicación que está pasando ahora mismo en el chat (una derivación en vivo, una fórmula que se acaba de construir juntos, algo que el usuario necesita ver ya para seguir la conversación)?** → múéstrala TAMBIÉN con el widget de visualización (HTML con MathJax vía CDN `mathjax@3/es5/tex-svg.js`, `tex: { inlineMath: [['$','$']], displayMath: [['$$','$$']] }`), en el mismo turno, sin que el usuario tenga que pedirlo.
  - **Si ambas preguntas dan sí (lo normal cuando se introduce una fórmula nueva importante), haz las dos cosas en el mismo turno:** la guardas en el archivo de la clase Y la muestras con el widget. Guardar en el archivo NUNCA reemplaza mostrarla en vivo si el momento lo pide — son complementarias, no alternativas.
  - Solo omite el widget si la fórmula es un aparte menor que no es el foco de la explicación de ese momento (aunque sí valga guardarla en el archivo para después).
  - Solo omite el archivo si es una fórmula desechable de un solo uso, sin valor de repaso futuro (raro).
  - Para expresiones triviales (un exponente, una raíz, una fracción simple) basta texto plano/Unicode en el chat (`x^2`, `√`, `a/b`, `2^2 = 4`) — ahí no hace falta ni archivo ni widget.
  - Ante la duda de si conviene mostrar el widget: hazlo. El costo de un widget de más es bajo; el costo de una fórmula que el usuario no puede leer en medio de la explicación rompe el flujo de la clase.
  - **Excepción — handoff hacia otro agente (`_protocolos/handoff.md`):** todo lo anterior aplica a esta sesión de Claude Code, que no renderiza LaTeX en el chat. Pero el prompt de SALIDA de un handoff no se queda en este chat: el usuario lo copia y lo pega en otra sesión externa (Claude web/app, ChatGPT, un agente local, etc. — de texto o de voz), que sí renderiza LaTeX nativamente sin necesidad de widget. Por eso, **dentro del bloque copiable del handoff de SALIDA, sí puedes usar LaTeX normal (`$...$`, `$$...$$`)** si el tema lo requiere — ahí no es un error, es el formato correcto para el agente destino. Esta excepción es solo para ese bloque específico; el resto del chat (todo lo que no sea el contenido a copiar) sigue la regla normal.
- Nunca asumas el nivel del usuario — léelo de `progreso.json`, `dificultades.json` y `glosario.json`.
- Prioriza las dificultades sin resolver si son prerrequisito del tema actual.
- Si el usuario pregunta "¿por dónde empiezo?", lee todos los `progreso.json` y recomienda según las dependencias declaradas en cada `README.md` de curso.
- Si el usuario pide un repaso general, revisa todos los `dificultades.json` con `resuelto: false` y agrupa por `tema_id`.
- Actúa como tutor paciente: explica, pregunta, verifica comprensión. El objetivo no es terminar el temario rápido sino que el usuario comprenda de verdad.
- No avances al siguiente tema si quedan dificultades sin resolver del tema actual que sean prerrequisito directo.

---

## Estructura del proyecto

```
estudios/
├── README.md                     ← Este archivo (algoritmo de arranque + protocolos). Fuente única de verdad.
├── CLAUDE.md                     ← Puntero que Claude Code carga automáticamente al iniciar; solo redirige a README.md (no duplica contenido).
├── .gitignore                    ← Ignora _backups/ y archivos temporales típicos
├── objetivo.json                 ← Se genera en el setup inicial (no existe al principio). Contiene el presupuesto global de horas y el array "objetivos": cada uno con su "estado" (activo/latente/completado), su "prioridad" ordinal, sus "hitos" y sus "cursos_requeridos" (muchos-a-muchos: un curso puede servir a varios objetivos).
├── configuracion.json            ← Ajustes del proyecto (ej: modo una clase por sesión). Se crea con defaults si falta.
├── historial.json                ← Registro APPEND-ONLY de sesiones (fecha/hora, duración real, tipo de clase, curso, temas). Única fuente de verdad sobre cuánto tiempo se estudia; alimenta las validaciones de deadline y el comando `estado`. Nunca se sobreescriben entradas.
├── _protocolos/
│   ├── entrevista.md             ← Protocolo del PRIMER arranque (no hay objetivo.json)
│   ├── expansion.md              ← Protocolo para AGREGAR nuevos objetivos (ya hay uno completo)
│   ├── handoff.md                ← Protocolo de puente entre agentes/sesiones (texto o voz)
│   ├── configuracion.md          ← Comandos para activar/desactivar reglas (ej: un tema por sesión)
│   └── utilidades.md             ← Comandos rápidos: mostrar estado, generar backup
├── _recursos/
│   └── herramientas.md           ← Guía detallada de herramientas externas (WolframAlpha, Desmos, etc.) — con protocolo de actualización
├── _backups/                     ← Backups locales generados con el comando `backup` (opcional; ignorado por git)
├── _plantillas/                  ← Plantillas para generar cursos, clases y archivos
│   ├── plantilla-objetivo.json
│   ├── plantilla-configuracion.json
│   ├── plantilla-historial.json
│   ├── plantilla-temario.json
│   ├── plantilla-progreso.json
│   ├── plantilla-glosario.json
│   ├── plantilla-dificultades.json
│   ├── plantilla-README-curso.md
│   └── plantilla-README-clase.md
└── NN-curso/                     ← Un directorio por curso (generados en setup)
    ├── README.md                 Objetivo del curso, prerrequisitos externos
    ├── temario.json              Árbol de temas del curso (mapa estático)
    ├── progreso.json             Estado por tema (dinámico, referencia por tema_id)
    ├── glosario.json             Términos conocidos por el usuario (con nivel_dominio)
    ├── dificultades.json         Dificultades del usuario (con tipo y estrategia_que_ayuda)
    └── clases/
        └── NN-tema/
            └── README.md         Apuntes y ejercicios de esa clase
```

### Valores de `estado` en `progreso.json` (FLUJO, no conocimiento)
- `"no_iniciado"` — tema no tocado aún
- `"en_progreso"` — clase abierta, sin cerrar (la rama (A) del PASO 5.5 la retoma)
- `"completado"` — las clases activas sobre el tema terminaron y pasó a la escalera de repaso. **NO es una medida de conocimiento** (esa es `nivel_alcanzado`): un tema puede estar `completado` con `nivel_alcanzado: "base"`. La doneness real es derivada y relativa al objetivo (`nivel_alcanzado >= nivel_requerido`). Ver "El conocimiento se mide en DOS dimensiones".

### Campos de cada tema en `progreso.json`
- `tema_id` — referencia al `id` del `temario.json`
- `estado` — uno de los tres de arriba (FLUJO)
- `nivel_alcanzado` — PROFUNDIDAD real y verificable: `null` → `"base"` → `"intermedio"` → `"avanzado"`. Solo sube cuando el usuario cumple el `criterio_dominio` de ese nivel, nunca por gastar sesiones. Lo escribe únicamente el "Protocolo al CERRAR una clase". Ver "El conocimiento se mide en DOS dimensiones".
- `ultima_sesion` — ISO 8601 con hora (`YYYY-MM-DDTHH:MM`), o `null` si nunca se tocó
- `sesiones_dedicadas` — contador de sesiones del tema
- `nivel_repaso` — 1-5, índice en la escalera de repaso (DURABILIDAD). `null` mientras `nivel_alcanzado` sea `null` (aún no llegó a `base`)
- `fecha_proximo_repaso` — `YYYY-MM-DD` de cuándo vuelve a entrar. `null` mientras `nivel_alcanzado` sea `null`

### Tipos válidos de dificultad en `dificultades.json`
- `"conceptual"` — no entiende el concepto en sí
- `"operativa"` — entiende pero se equivoca ejecutando (signos, distribución, aritmética)
- `"notacion"` — se confunde con la simbología o cómo escribir
- `"aplicacion"` — sabe el tema aislado pero no reconoce cuándo aplicarlo en un problema

### Niveles de dominio en `glosario.json`
- `"reconoce"` — lo entiende cuando lo ve pero no lo produce solo
- `"solido"` — lo aplica y explica sin dudar
- `"vacilante"` — a veces lo confunde con otro concepto

---

## Futuras mejoras (ideas aparcadas, no implementadas)

> Cosas que se evaluaron, se decidió que valen la pena, y se dejaron fuera del alcance del momento. **Nada de aquí está implementado** — no asumas que existe. Si vas a implementar una, bórrala de esta lista y documéntala donde corresponda.

*(Ninguna pendiente. Aquí vivieron el historial de sesiones y el problema de cursos compartidos; ambos se resolvieron el 2026-07-16 — ver `historial.json`, y "Cursos compartidos" y "El presupuesto: una cola con prioridad" en "Objetivos paralelos".)*

---

## Curso de referencia del template

El directorio `_ejemplo-algebra/` contiene un ejemplo completo de cómo debe verse un curso bien generado, incluyendo `temario.json`, `progreso.json`, `glosario.json` y `dificultades.json` poblados. Sirve como patrón visual cuando el agente genere nuevos cursos en el setup.

**Importante:** este directorio empieza con guión bajo para indicar que es metadata del template (igual que `_protocolos/` y `_plantillas/`) y NO un curso real del usuario. Se elimina en el Paso 0 del setup inicial (ver `_protocolos/entrevista.md`).
