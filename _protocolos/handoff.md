# Protocolo de handoff (puente entre agentes / sesiones)

> Permite que el usuario pase el contexto de estudio de una sesión con este agente (típicamente Claude Code) a otra sesión externa con otro agente (Claude web/app, ChatGPT, un agente local, etc. — de texto o de voz, cualquiera de las dos) y vuelva con un resumen que actualice los archivos del proyecto.

## Referencia rápida de comandos

| Comando completo | Alias corto | Fase | Qué hace |
|---|---|---|---|
| `generar un handoff` | `hndff` | SALIDA | Genera un prompt copiable para llevar el estudio de UN tema a una sesión externa (otro agente, de texto o de voz) |
| *(pegar bloque cuya primera línea es `=== HANDOFF-RESUMEN ===`)* | — | VUELTA | Procesa el resumen de la sesión externa y actualiza los archivos del proyecto |

**Regla:** el agente acepta indistintamente la forma completa y el alias corto. Si el usuario dice algo parecido pero no idéntico a ninguno de los dos, pídele que use la forma exacta — no invoques el protocolo por conjetura. Nuevos tipos de handoff se agregarán a esta tabla en el futuro con su propio comando y alias. Ningún comando usa `/` (ver convención en el `README.md` raíz).

Este protocolo tiene dos fases:
1. **Handoff de SALIDA** → generar un prompt con contexto para llevar a la otra sesión.
2. **Handoff de VUELTA** → recibir el resumen de esa sesión y actualizar los archivos.

---

## Cuándo aplicar este protocolo

### Comando de invocación (SALIDA)

Este protocolo se activa **únicamente** cuando el usuario escribe uno de los comandos explícitos siguientes. NO lo actives por paráfrasis vagas o intenciones implícitas — pide confirmación con el comando exacto si el usuario dice algo similar pero no idéntico.

| Comando exacto | Tipo de handoff |
|---|---|
| `generar un handoff` (alias `hndff`) | Handoff hacia una sesión externa con otro agente, de texto o de voz (app de Claude, ChatGPT, agente local, etc.) |

*(Nuevos tipos de handoff se agregarán aquí en el futuro con su propio comando específico. No inventes tipos que no estén en esta tabla.)*

### Detección de VUELTA

El usuario pega un bloque de texto que empieza con la línea exacta:

```
=== HANDOFF-RESUMEN ===
```

Esa línea es el marcador; reconócelo automáticamente sin necesidad de comando adicional.

### Regla clave: UN handoff = UNA clase (pero puede ser parcial)

Un handoff cubre **exactamente una clase**, en el sentido definido en "Anatomía de una clase" del `README.md` raíz. Una clase tiene:

- **Un `tema_principal`** — el `tema_id` que se va a trabajar. Es el único que puede cambiar de `estado` y `porcentaje`.
- **Cero o más `temas_intercalados`** — `tema_id` ya `completado` que entran solo como ejercicios de repaso (el ~30% del bloque, ver Método 3 del README). De estos **solo se actualiza la escalera de repaso** (`nivel_repaso`, `fecha_proximo_repaso`, `ultima_sesion`), nunca su `estado` ni su `porcentaje`.

Nunca generes un handoff con más de un `tema_principal`, ni permitas que el resumen de vuelta toque temas que no fueron declarados en el prompt de salida.

Motivación: mantiene el seguimiento limpio y el flujo predecible, sin romper el intercalado. Si el usuario quiere trabajar dos temas nuevos en su sesión externa, debe generar un handoff por cada uno y procesar cada resumen por separado.

Si el usuario pide un handoff que cubra más de un tema nuevo (ej: "quiero llevar todo el curso a otra sesión"), rechaza amablemente y sugiérele elegir un tema principal concreto.

> **Nota de cambio (julio 2026):** esta regla decía "UN handoff = UN tema" y ordenaba rechazar cualquier resumen multi-tema. Eso era incompatible con el intercalado: una clase normal toca el tema nuevo *más* ejercicios de 2-3 temas viejos, así que todo handoff devolvía un resumen que la fase de VUELTA descartaba a medias — perdiendo justo lo que alimenta la escalera de repaso. El límite ahora está en la clase, no en el tema.

**Resumen parcial permitido dentro del mismo tema.** El tema puede no completarse en la sesión externa (el usuario se cansa, tiene que salir, no llega al Feynman). En ese caso, el resumen es válido igual — reflejará avance parcial (`estado_sugerido: en_progreso`, porcentaje bajo, dificultades sin resolver). El siguiente handoff sobre este mismo tema se genera igual, y su prompt de SALIDA incluye automáticamente el estado actual (que ya refleja el avance parcial previo). Nunca fuerces al usuario a "terminar el tema" para poder cerrar un handoff.

---

## FASE 1 — Handoff de SALIDA

### Paso 1.1: Determinar el alcance (UNA clase)

Pregunta al usuario:
1. **¿Sobre qué tema va la sesión externa?** Debe ser **un único `tema_id`** — el `tema_principal`. Por defecto, el tema que decidiría el PASO 5.5 del `README.md` raíz (córrelo si no lo hiciste ya en esta sesión). Si el usuario propone más de un tema nuevo o algo demasiado amplio ("todo cálculo", "el curso completo"), rechaza y pide que elija uno solo. Ofrece hacer handoffs sucesivos si necesita cubrir varios.
2. **¿Qué tipo de sesión será?** (Ej: explicación conceptual, repaso, ejercicios verbales, Feynman del tema ya estudiado). Esto afecta las instrucciones que damos al agente destino.
3. **¿Con qué agente y en qué modalidad va a estar?** (Claude app/web en modo texto o voz, ChatGPT, un agente local, etc.). Afecta limitaciones a advertir — si la modalidad es de voz, un agente de voz no verá LaTeX ni gráficos.

**Los `temas_intercalados` no se preguntan: se calculan.** Son los temas `completado` con `fecha_proximo_repaso <= hoy`, ordenados del más vencido al menos vencido; toma los 2-3 primeros. Si no hay ninguno vencido, elige 1-2 temas ya vistos relacionados con el principal (prerrequisitos internos, sobre todo) — el intercalado no es solo para el repaso. Menciónale al usuario cuáles metiste y por qué, en una línea.

**Excepción:** si la clase es de tipo `repaso` (rama B del PASO 5.5), no hay tema nuevo. En ese caso el `tema_principal` es el tema vencido más crítico y el resto van como intercalados, y el prompt debe decirle al agente destino que es una sesión de repaso, no de contenido nuevo.

**Validaciones antes de generar el prompt:** todos los `tema_id` (principal e intercalados) deben existir en un `temario.json` del proyecto. Si alguno no existe, avisa y pide corrección. Los intercalados además deben estar en `estado: "completado"` — no se intercala un tema que nunca se estudió.

### Paso 1.2: Recolectar el contexto

Lee de los archivos:
- `objetivo.json` → **el/los objetivo(s) que requieren este curso** (aquellos en cuyo `cursos_requeridos` aparece) y su motivación. Un curso puede servir a varios: si es el caso, menciónalos todos y usa el hito más cercano como la fecha que aprieta. Con objetivos en paralelo, no mezcles: una clase de inglés no se motiva con el objetivo de la UNI.
- README del curso + `temario.json` → objetivo del curso, tema y sus `objetivos_aprendizaje` y `criterio_dominio`.
- `progreso.json` → estado y sesiones dedicadas al tema principal; `nivel_repaso` y `fecha_proximo_repaso` de los intercalados.
- `temario.json` → `objetivos_aprendizaje` de cada tema intercalado (el agente destino necesita saber qué evaluar en ellos).
- `glosario.json` → filtra por `tema_id` del tema principal, de sus prerrequisitos internos y de los intercalados.
- `dificultades.json` → filtra dificultades con `resuelto: false` relacionadas al tema principal, sus prerrequisitos o los intercalados.
- Últimos `clases/NN-tema/README.md` del tema principal si existen → últimos apuntes.
- `clases/NN-tema/README.md` de los intercalados si existen → de ahí salen ejercicios de repaso mucho mejores que inventándolos desde el temario.

### Paso 1.3: Generar el prompt de salida

Escribe el prompt completo al usuario **en un solo bloque copiable** (dentro de un code fence), listo para pegar en la otra sesión. Este es el formato exacto:

> **Nota sobre LaTeX:** la regla general del proyecto (ver `README.md`, "Notación matemática") prohíbe LaTeX crudo en el chat de Claude Code porque no renderiza ahí. Esa regla NO aplica dentro de este bloque copiable: el destino es otro agente externo (Claude web/app, ChatGPT, etc., en modo texto o voz) que sí renderiza LaTeX nativamente. Si el tema trabajado necesita fórmulas, escríbelas con LaTeX normal (`$...$`, `$$...$$`) dentro del bloque sin problema.

````markdown
# Contexto de mi sesión de estudio

Estoy usando un plan de estudio estructurado como tutor apoyado por IA. Vengo de otra sesión y quiero continuar contigo aquí. Al final de nuestra conversación necesito un resumen en un formato específico para poder actualizar mi seguimiento.

## Alcance de esta sesión (importante)

Vamos a trabajar **una clase** de mi curso de [Nombre del curso]. Una clase tiene esta forma:

- **Tema principal: `[tema_id]`** — es el contenido nuevo de hoy y el único tema que puede avanzar.
- **Temas de repaso intercalado: `[tema_id_1]`, `[tema_id_2]`** — temas que ya estudié y toca refrescar. NO los expliques desde cero: solo métemelos como ejercicios mezclados dentro del bloque de práctica (apunta a ~30% de los ejercicios), sin agruparlos ni anunciarlos por adelantado. Quiero tener que darme cuenta yo solo de qué tipo de problema es cada uno.

No abras ningún tema nuevo que no sea el principal, aunque yo te lleve por la conversación hacia otros. Si nos desviamos hacia contenido nuevo distinto (aunque sea relacionado), recuérdame que este handoff es solo para esta clase y sugiéreme cerrarla con el resumen para hacer otro handoff después.

## Sobre mí y mi objetivo
- **Objetivo(s) a los que sirve este curso:** [objetivo_final de cada objetivo que lo requiere; si son varios, dilo — saber que un tema cuenta para dos metas a la vez motiva]
- **La fecha que aprieta:** [el hito no condicional más cercano entre esos objetivos: nombre y fecha]
- **Motivación:** [motivacion del objetivo que da la urgencia efectiva]
- **Nivel:** [nivel_actual_al_inicio de ese objetivo, complementado con progreso real]

## Curso actual: [Nombre del curso]
[objetivo_del_curso del temario]

## Tema principal de esta clase: [Nombre del tema]
- **ID del tema:** [tema_id]
- **Descripción:** [descripcion del tema]
- **Objetivos de aprendizaje:**
  - [objetivo 1]
  - [objetivo 2]
- **Criterio de dominio esperado:** [criterio_dominio]
- **Estado actual:** [estado, porcentaje]%, [sesiones_dedicadas] sesiones dedicadas hasta ahora.

## Temas de repaso intercalado

[Por cada tema intercalado. Si no hay ninguno, escribe "Ninguno esta vez — no intercales repaso."]

### [Nombre del tema] (`[tema_id]`)
- **Lo estudié:** hace [N] días. **Objetivos que debería seguir cumpliendo:** [objetivos_aprendizaje del temario]
- **Qué vimos:** [2-3 líneas de los apuntes de esa clase, si existen]
- **Mételo como:** 1-2 ejercicios sueltos dentro del bloque de práctica, sin explicación previa. Es una prueba de recuperación, no una re-enseñanza. Si fallo, ahí sí explícame — y anótalo en el resumen.

## Contexto temporal
- **Última sesión de este tema:** [ultima_sesion del progreso.json, o "es la primera sesión"]
- **Días transcurridos desde entonces:** [calcular: hoy - ultima_sesion]
- **Recomendación de arranque:** [si días > 7 → "empezar con un repaso activo antes de contenido nuevo"; si 2-7 → "recuperación activa breve y avanzar"; si ≤1 → "continuar directo desde donde quedamos"]

Aplica esta recomendación al iniciar la conversación.

## Lo que ya sé del tema y prerrequisitos
[Lista de términos relevantes del glosario, formato: "Término: definición corta. (nivel: solido/reconoce/vacilante)"]

## Lo que me cuesta (dificultades activas)
[Lista de dificultades relevantes con resuelto:false, formato: "Tipo (conceptual/operativa/etc): descripción. Error típico: [error]. Estrategia que ha ayudado: [estrategia o 'aún ninguna']"]

## Últimos apuntes de clase
[Resumen de 3-5 líneas de los últimos apuntes de las carpetas de clases del tema, si existen]

## Qué quiero de esta sesión
[Lo que dijo el usuario en Paso 1.1 respuesta 2 — tipo de sesión]

## Métodos que aplico en mi estudio
Aplico estas técnicas y me gustaría que las mantengas:
- **Recuperación activa**: hazme preguntas primero, no me sueltes explicaciones sin verificar qué recuerdo.
- **Interrogación elaborativa**: cuando explique algo, pregúntame "¿por qué?" antes de confirmar.
- **Feynman**: al final, pídeme que te explique el tema con mis palabras como si te lo enseñara.
- **Ejercicios variados**: si me das ejercicios, mezcla tipos, no me des cinco del mismo tipo seguidos.

## FORMATO OBLIGATORIO DEL RESUMEN AL FINAL

Cuando termine nuestra conversación, dame un mensaje que empiece exactamente con la línea `=== HANDOFF-RESUMEN ===` y que siga esta estructura Markdown. Voy a copiarlo tal cual en mi otra sesión para actualizar mi seguimiento — no lo modifiques ni agregues texto fuera de este bloque.

```markdown
=== HANDOFF-RESUMEN ===

## Metadata
- Fecha y hora de inicio: YYYY-MM-DDTHH:MM
- Tema principal: [tema_id principal que te pasé arriba]
- Temas intercalados: [los tema_id de repaso que te pasé arriba, separados por coma, o "ninguno"]
- Duración real: X minutos (pregúntamelo si no lo sabes con certeza — este dato alimenta mi seguimiento de horas, así que una estimación tuya a ojo no sirve)

## Progreso del tema principal
- Estado sugerido: [en_progreso | completado]
- Porcentaje estimado: X (0-100)
- Puntos que quedaron sin cubrir: [lista o "ninguno"]

## Repaso intercalado
[Una línea por cada tema intercalado, formato: "- tema_id | resultado: bien / mal | qué pasó en una frase".
"bien" = resolvió sus ejercicios sin ayuda. "mal" = falló, dudó mucho, o necesitó que le recordaras el método.
Sé estricto con esto: es lo que decide si el tema se da por consolidado o vuelve a la cola de repaso mañana.
Si un tema intercalado no llegó a tocarse en la sesión, escribe "- tema_id | resultado: no se tocó".
Si no había intercalados, escribe "no hubo".]

## Recuperación activa inicial
[Qué recordé al empezar la sesión, sin apoyo de apuntes, sobre el tema anterior o los prerrequisitos de este tema — qué estaba sólido y qué hueco se detectó. Si no se hizo recuperación activa al inicio (ej. sesión que retoma directo un tema ya empezado), escribe "no se hizo esta vez"]

## Términos nuevos aprendidos o afianzados
[Uno por línea, formato: "- término | definición corta en palabras del usuario | nivel_dominio: reconoce/solido/vacilante"]

## Dificultades nuevas detectadas
[Uno por línea, formato: "- tipo (conceptual/operativa/notacion/aplicacion) | descripción | error_tipico"]

## Dificultades resueltas (de las que traía)
[Uno por línea: "- descripción original | estrategia_que_ayudo"]

## Dificultades trabajadas pero no resueltas
[Uno por línea: "- descripción | qué se intentó"]

## Apuntes clave de la sesión
[NO te limites a bullets cortos. El objetivo es que yo pueda repasar el tema completo desde este resumen sin haber estado en la conversación ni volver a leerla — el mismo nivel de detalle que un apunte de clase normal. Incluye: cada definición o regla vista (con su fórmula si aplica), al menos un ejemplo numérico resuelto por regla/fórmula, y las derivaciones o "por qué funciona así" que se construyeron en la sesión (no solo el resultado final). Usa sub-bloques con encabezado si se vieron varios sub-temas distintos. Usa LaTeX normal (`$...$`, `$$...$$`) donde haga falta — este bloque sí lo renderiza tu agente actual.]

## Ejercicios trabajados
[Por cada ejercicio: el enunciado completo, luego mi respuesta citada de forma TEXTUAL/VERBATIM (tal cual la escribí o dije, con errores de tipeo incluidos, sin resumir ni parafrasear), y tu propia anotación de qué reveló esa respuesta. Si no se trabajaron ejercicios, escribe "no se trabajaron ejercicios"]

## Feynman del usuario
[La consigna EXACTA que me pediste para el Feynman, y mi respuesta citada de forma TEXTUAL/VERBATIM — tal cual la dije o escribí, sin resumir ni parafrasear, incluso con errores de tipeo o de lenguaje. No la comprimas a unas pocas líneas: cópiala completa. Si no llegamos al Feynman, escribe "no se llegó"]

## Próximo paso sugerido
[Una línea con qué recomiendo trabajar en la siguiente sesión]

=== FIN-HANDOFF-RESUMEN ===
```

Un solo `=== HANDOFF-RESUMEN ===` por conversación, y siempre referido a la clase que te pasé arriba: el tema principal `[tema_id]` más sus temas de repaso intercalado. Si por algún motivo terminamos hablando de un tema nuevo distinto, no lo incluyas en el resumen — avísame que lo dejaremos para otro handoff.

**Resumen parcial es perfectamente válido.** Si tengo que cortar la sesión sin haber completado el tema (cansancio, tiempo, se puso complicado, lo que sea), pídeme el resumen de todos modos con lo que sí llegamos a cubrir — `Estado sugerido: en_progreso`, porcentaje realista, dificultades sin resolver listadas. No me exijas terminar el tema para cerrar; prefiero un resumen parcial fiel a que no haya ningún resumen. Puedo hacer otro handoff después para retomar.
````

Después de generar el bloque, dile al usuario:
- "Cópialo y pégalo en tu otra sesión."
- "Al terminar, tráeme lo que empieza con `=== HANDOFF-RESUMEN ===` y lo proceso para actualizar tus archivos."

### Paso 1.4: Registrar que hay un handoff pendiente

Añade en `objetivo.json` un campo temporal `handoff_pendiente`:

```json
"handoff_pendiente": {
  "fecha_salida": "YYYY-MM-DDTHH:MM",
  "tema_principal": "id-del-tema",
  "temas_intercalados": ["id-tema-viejo-1", "id-tema-viejo-2"],
  "curso": "nombre-del-curso"
}
```

Esto sirve para: (a) recordarle al usuario al inicio de la siguiente sesión que hay un handoff sin procesar, (b) validar que el resumen que traiga corresponde a la clase esperada, (c) saber qué temas tenía permiso de tocar ese handoff.

---

## FASE 2 — Handoff de VUELTA

### Paso 2.1: Detección y validación del alcance de la clase

El usuario pega un bloque que empieza con `=== HANDOFF-RESUMEN ===`. Reconócelo automáticamente y aplica este paso sin preguntar.

Validaciones antes de procesar:

1. **Verificar que el resumen se refiere a UNA clase.** Lee la sección "Metadata": debe haber un único `Tema principal`. La lista `Temas intercalados` puede tener varios — eso es normal y esperado, no un error.

2. **Verificar que nada se salió del alcance declarado.** Los únicos temas que este resumen puede tocar son el principal y los intercalados listados en Metadata. Si en el cuerpo hay actualizaciones vinculadas a un `tema_id` fuera de esa lista, **detén el procesamiento** y avisa: "Este resumen toca temas que no estaban en el alcance del handoff ([lista]). Voy a procesar solo el tema principal y sus intercalados declarados, y descartar el resto. ¿Confirmas o prefieres corregir el resumen antes?"

3. **Verificar el reparto correcto de actualizaciones.** El tema principal es el único que puede cambiar `estado` y `porcentaje`. Si el resumen sugiere cambiar el estado de un tema *intercalado*, ignora ese cambio (de un intercalado solo se actualiza la escalera de repaso) y menciónalo al usuario en el reporte final.

4. **Verificar coincidencia con `handoff_pendiente`.** Si `objetivo.json` tiene `handoff_pendiente`, el `tema_principal` del resumen debe coincidir. Si no coincide, avisa: "El resumen es de un tema distinto al handoff pendiente ([tema_pendiente]). ¿Fue intencional?" y espera respuesta antes de continuar. Si los intercalados no coinciden exactamente, no es motivo de alarma — la sesión externa pudo no llegar a todos; procesa los que sí reporta.

5. **Verificar que todos los `tema_id` existen** en algún `temario.json` del proyecto. Si alguno no existe, avisa y no proceses ese.

### Paso 2.2: Revisión de coherencia antes de guardar (ruido de transcripción u otras inconsistencias)

**Antes** de escribir nada en los JSON o en el README de la clase, revisa el bloque completo buscando incoherencias internas — esto importa especialmente si la sesión fue por voz o transcripción, donde el ruido de dictado es frecuente.

Señales a buscar:
- Un resultado numérico en una cita del usuario que no cuadra con la operación que él mismo describe en esa misma respuesta, pero sí cuadra si se asume un error de dictado (ej. "doce" transcrito en vez de "dos", "MCB" en vez de "MCD").
- Un término técnico que no existe pero suena parecido a uno real que encaja en el contexto (ej. "algoritmo de utilidades" en vez de "algoritmo de Euclides").
- Contradicción entre la "Anotación" del agente que llevó la sesión externa y lo que literalmente dice la cita del usuario.

Qué hacer en cada caso:
1. **Ruido de transcripción evidente que no afecta el razonamiento** (el error está solo en cómo quedó escrito el número/palabra, no en el proceso que siguió el usuario): corrígelo únicamente en tu propia síntesis (la sección "Resumen / apuntes" que redactas tú). **Nunca alteres la cita textual/verbatim** de "Ejercicios trabajados" ni "Feynman de cierre" — van tal cual llegaron, ruido incluido, porque la regla de cita verbatim es para preservar la voz real, no para que el texto sea impecable. Sí debes dejar explícito en tu "Anotación del agente" de ese ejercicio que se detectó ruido de transcripción, para que quede claro que no fue un error conceptual real.
2. **Caso ambiguo, no intuitivamente resoluble** (no puedes decidir con confianza si es ruido de dictado o un error real de razonamiento del usuario): **no lo resuelvas por tu cuenta ni lo guardes todavía.** Muéstrale al usuario la cita exacta y tu duda concreta, y espera su respuesta antes de continuar con ese punto.
3. **Nunca dejes que un ruido de transcripción no detectado se filtre a `glosario.json` o `dificultades.json`** como si fuera contenido real — ni una definición corrupta por ruido de dictado, ni una dificultad nueva que en realidad es solo un tropiezo del dictado y no del razonamiento.

### Paso 2.3: Procesar el resumen

Extrae cada sección del bloque (ya revisada en el paso anterior) y actualiza los archivos:

**`progreso.json` — tema principal**
- Actualiza `estado` y `porcentaje` con lo sugerido.
- Incrementa `sesiones_dedicadas` en 1.
- Actualiza `ultima_sesion` con la fecha y hora del resumen (ISO 8601, `YYYY-MM-DDTHH:MM`). Si el resumen trajo solo la fecha sin hora, guárdala sin hora en vez de inventarla.
- Si pasó a `completado`, arma su repaso: `nivel_repaso: 1`, `fecha_proximo_repaso` = el día siguiente a la fecha del resumen.

**`historial.json`** — agrega **una entrada al final** (append, nunca sobreescribas) con `inicio` y `duracion_min` tomados de la Metadata del resumen, el `objetivo_id` del objetivo dueño del curso, `tema_principal`, `temas_intercalados`, y en `nota` el agente y modalidad de la sesión externa. Si el resumen no trajo duración, **pregúntasela al usuario** antes de escribir — no la inventes ni dejes el campo en null.

**`progreso.json` — temas intercalados** (de la sección "Repaso intercalado" del resumen)
- `resultado: bien` → sube `nivel_repaso` en 1 (tope 5) y recalcula `fecha_proximo_repaso` = fecha del resumen + el intervalo del nivel nuevo (ver la escalera en el `README.md` raíz).
- `resultado: mal` → pon `nivel_repaso: 1` y `fecha_proximo_repaso` = el día siguiente a la fecha del resumen. Registra además la dificultad correspondiente en `dificultades.json`.
- `resultado: no se tocó` → no cambies nada de ese tema.
- En todos los casos: **no toques su `estado`, `porcentaje` ni `sesiones_dedicadas`.** Sí actualiza su `ultima_sesion` si se tocó.

**`glosario.json`**
- Por cada término en "Términos nuevos aprendidos": si no existe, crea entrada nueva con la estructura completa (usando `tema_id` del resumen, `curso` correspondiente, `fecha_aprendido` = fecha resumen). Si ya existe, actualiza `nivel_dominio` y `fecha_ultimo_uso`.

**`dificultades.json`**
- Por cada nueva dificultad detectada: crea entrada con `id` incremental (`dif-NNN`), tipo, descripción, error_tipico, fecha_deteccion = fecha resumen, `resuelto: false`.
- Por cada dificultad resuelta: encuentra la entrada correspondiente por descripción, marca `resuelto: true`, llena `estrategia_que_ayuda`, incrementa `veces_repasado`, actualiza `fecha_ultimo_repaso`.
- Por cada dificultad trabajada pero no resuelta: encuentra la entrada, incrementa `veces_repasado`, actualiza `fecha_ultimo_repaso`.

**`clases/NN-tema-id/README.md`**
- Si no existe la carpeta de la clase, créala con la plantilla.
- Poblá "Recuperación activa inicial" con la sección homónima del resumen.
- Poblá "Resumen / apuntes" con los "Apuntes clave" del resumen. Esta sección debe quedar con la misma profundidad que un apunte de clase nativo (definiciones, fórmulas, ejemplos resueltos, derivaciones) — si el resumen recibido vino pobre en detalle (bullets cortos sin fórmulas ni ejemplos), reconstruye tú mismo esa profundidad a partir de lo que sí es verificable en "Ejercicios trabajados" y "Feynman del usuario" del resumen (son la fuente más rica y ya verbatim) antes de darlo por completo. No inventes contenido que no esté respaldado por esas dos secciones.
- Poblá "Ejercicios trabajados" y "Feynman de cierre" copiando esas secciones del resumen **tal cual** (cita textual del enunciado/consigna + respuesta verbatim del usuario), igual que exige el `README.md` del proyecto para estas dos secciones — no las resumas ni las parafrasees de nuevo aquí.
- En "Herramientas usadas" indica "sesión externa ([agente que usó], modalidad: texto/voz)".

### Paso 2.4: Limpiar el handoff pendiente

Elimina el campo `handoff_pendiente` de `objetivo.json`.

### Paso 2.5: Reporte al usuario

Muestra un resumen breve de lo actualizado:
```
Handoff procesado:
- Tema principal [X] avanzó de Y% a Z%.
- Repaso intercalado: [tema A] subió a nivel N (próximo: fecha); [tema B] volvió a nivel 1 (próximo: mañana).
- N términos nuevos en glosario.
- M dificultades nuevas registradas, K resueltas.
- Clase [fecha] agregada en clases/[NN-tema]/.

Próximo paso sugerido: [del resumen].
```

Y pregunta si quiere continuar con la próxima sesión ahora o dejarlo aquí.

---

## Notas importantes

- **El formato del bloque de vuelta es estricto por diseño**: si el agente destino no lo respeta al 100%, no importa — parsea con flexibilidad. Si algo falta, pregúntale al usuario en vez de asumir.
- **No sobreescribas dificultades como resueltas sin confirmación** si el resumen es ambiguo. Es mejor conservador aquí.
- **Este protocolo funciona en ambas direcciones**: el usuario puede empezar en una sesión externa (texto o voz) y traer contexto a Claude Code también, siguiendo el mismo formato.
- **Si el usuario cambia mucho de agente**, considera sugerirle usar directamente Claude Code como fuente de verdad para no perder detalles en cada round-trip.
