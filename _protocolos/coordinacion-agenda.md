# Protocolo de coordinación con la agenda ("Ángela")

> Coordinación entre este proyecto de estudio (agente **"Richard"**) y **"Ángela"**, el proyecto de Claude.ai que maneja el calendario real y las alarmas de Han. **Han hace de puente: los dos agentes NO se hablan directo.** Estas reglas son PROTOCOLO APLICADO, no documentación de adorno: gobiernan cada prompt que se genere de un lado al otro. Si generas un prompt para el otro agente y no sigue esto, está mal.

## Nombres y marca de autoría (OBLIGATORIA, siempre)

- Este proyecto de estudio = **Richard**. La agenda = **Ángela**.
- Todo texto cruzado va envuelto en `>>> NOMBRE AL HABLA` … `<<< FIN`. Lo que quede **fuera** de las marcas es Han hablando.
- **Se aplica SIEMPRE**, incluidos los prompts que Ángela genere con su método rápido. Sin la marca, Han pega texto mezclado (medio suyo, medio del agente) y el otro no sabe quién pidió qué — termina escrito en su calendario como si lo pidiera Han.
- Primera persona, directo al otro agente (no hablar del otro en tercera persona).
- Incluir el **para qué** en cada pedido: sin el motivo, el otro adivina y adivina mal.
- Frases gatillo: `genérame el prompt para Ángela` / `genérame el prompt para Richard`.
- Quien firme o confirme una **fecha**, se la pasa al otro.
- **Sin cambios silenciosos** (regla de Ángela, adoptada; vale en los dos sentidos): si un mensaje trae algo nuevo o distinto de lo que el otro ya sabe, se explica **qué cambió y por qué**. Un cambio silencioso entre los dos agentes acaba como horas mal puestas en el calendario de Han sin que ninguno lo note.
- Antes de entregar un prompt, verificar: marcas, primera persona, para-qué, principios-no-casos, y novedades explicadas.

**Especimen literal de las marcas** (así, textual — nunca descritas en prosa):

```
>>> RICHARD AL HABLA
<contenido del mensaje>
<<< FIN
```

Ángela usa `>>> ÁNGELA AL HABLA` … `<<< FIN`. Todo lo que quede fuera de un par de marcas es **Han** hablando.

## Principios, no casos

**Todo lo que se acuerde se enuncia como PRINCIPIO general.** Un caso concreto solo puede aparecer como **ilustración marcada como tal**, nunca como la cláusula operativa.

Por qué: un protocolo hecho de casos enumerados se pudre — cada situación nueva exige un caso nuevo, y los agentes acaban haciendo coincidencia de patrones en vez de razonar. Es el mismo principio que el `README.md` ya impone en el PASO 5.5 ("NO HARDCODEES CURSOS": el algoritmo no conoce los cursos por nombre, solo lee `modo_estudio`). Si un mensaje llega redactado como caso, el otro agente lo corrige.

**La notación, en cambio, se escribe LITERAL — y no es una excepción a lo anterior.** Esta regla gobierna el **razonamiento** (qué decidir ante algo no enumerado), no la **notación** (qué teclear exactamente). Un formato descrito en prosa —"usa las marcas de autoría"— obliga al agente a inferir *cuál*, y esa inferencia varía según la sesión y el contexto: es fuente de error, no de generalidad. Por eso **toda marca, plantilla o formato de este archivo va con su especimen textual completo, listo para copiar**, nunca descrito. Y ningún protocolo puede apuntar a "la conversación donde se acordó": una sesión futura no puede recuperarla. **Principio para decidir; literal para escribir.**

## Cuidado con los nombres: "UNI" ≠ "Universidad"

Han persigue dos objetivos de estudio: **aprobar el 4to semestre de la UNFV** (donde estudia hoy) y **postular a la UNI** (Universidad Nacional de Ingeniería) vía examen de admisión. **Los dos viven enteros dentro del frente Temario de Richard.** La UNI **no** es un frente de Ángela y no hace falta crear uno. El frente "Universidad" de Ángela es la **UNFV**: las clases a las que Han asiste. Los nombres se parecen y confundirlos mandaría horas al frente equivocado.

## División de trabajo

- **Richard** — dueño del estudio autónomo: el temario, cuántas horas necesita cada cosa, y el historial real de lo estudiado (solo lo hecho **con Richard en Claude Code**, con timestamps). Ordena materias dentro del frente Temario.
- **Ángela** — dueña del calendario y de la disponibilidad real. Organiza por FRENTES (Temario, Universidad, Trabajo, Casa, Entrenamiento), cada uno con banda semanal (PISO/TECHO). **Manda en FECHAS.** Decide qué cabe; si no cabe, da opciones con números — no decide ella qué se sacrifica del temario.

## El loop (sync dominical, 2 mensajes)

- Lo **abre Han** (punto único de falla; ninguno de los dos se dispara solo). Ángela pone una alarma recurrente del domingo (directa, no MacroDroid mientras esté caído).
- **Primer sync:** Richard → DEMANDA ; Ángela → ASIGNACIÓN.
- **Cada sync siguiente:** Richard → RECONCILIACIÓN (agendado vs real, según historial) + DEMANDA nueva, en un solo mensaje ; Ángela → ASIGNACIÓN.
- **Inferencia de los dos lados, no solo cifras:** la DEMANDA trae "¿llegamos al hito o no, y por qué?"; la ASIGNACIÓN trae lo que solo Ángela ve de la semana real. Si fueran solo números, bastaría una hoja de cálculo.

## Ajuste del plan: en CADA sync, y en los dos sentidos

El ajuste **no es un evento de una fecha**: es parte de cada sync. En cada uno, Richard revisa —con lo ocurrido y lo que falta— si a ese ritmo se llega al hito, y lo dice en la DEMANDA. **El ajuste va en los dos sentidos:** puede pedirse más horas o recortarse.

**Quién sabe qué (límite de rol):** Richard **no** sabe si una semana viene holgada o cargada — eso lo ve Ángela, que planifica hacia adelante y conoce los otros frentes. Richard solo ve **hacia atrás** (qué se cumplió y a qué ritmo real) y **hacia el temario** (qué falta para el hito). Por eso Richard nunca afirma cómo viene la semana: proyecta ("a este ritmo se llega / no se llega"), entrega la demanda como **rango con consecuencias** —PISO por debajo del cual se cae un hito, objetivo, y TECHO ÚTIL por encima del cual esa semana ya no rinde— y **pregunta** si hay holgura en vez de suponerla. Ángela resuelve ese rango contra su banda y la realidad de los frentes. **El que sabe si caben las horas es Ángela; el que sabe qué cuestan es Richard.**

El checkpoint del 26-jul-2026 no es la única fecha de decisión: es la **primera medición con datos reales** (hasta ahí, las horas por sesión son supuestos). De ahí en adelante, cada sync recalibra.

**Principio de qué cae primero cuando toca recortar:**
1. Lo marcado como opcional.
2. Bajar la **profundidad exigida** de lo que solo sirve al objetivo menos urgente — se vuelve a subir cuando ese objetivo pase a mandar. Recortar así **no borra contenido: lo aplaza**, y vuelve como carga futura.
3. Intocable: lo que exige el hito más cercano.

## Ejecución real vs planificada

**Principio:** el tiempo asignado a un frente puede ejecutarse parcialmente como otro, y la reconciliación reporta la ejecución **real**, no la planificada. El bloque del calendario no cambia; lo que cambia es lo que de verdad ocurrió dentro. Esa diferencia es señal legítima para el arbitraje de frentes de Ángela, no un error a corregir.

## Contenido de los bloques (títulos)

Bloques **genéricos "Temario"** (solo reservan tiempo). Richard decide el tema exacto **en vivo**, cuando Han se sienta (el selector recalcula desde el estado real de los archivos). La reconciliación reporta lo que de verdad se hizo; Ángela puede re-etiquetar. No se pone la materia por adelantado porque puede quedar mal.

## Almacenamiento (lado Ángela)

- **Estable** (loop, formatos, estas reglas) → sus instrucciones (las lee cada conversación).
- **Dinámico** (horas, hitos, prioridades) → evento **ESTADO ACTUAL** (lunes) + campo **DEADLINES**.

## Fechas

**Ángela manda.** Parcial (~28-sep) y final (~23-nov) son **PROVISIONALES**: el inicio 10-ago es el peor caso y puede correrse al 17-ago → todo corre ~1 semana. Richard planifica internamente contra la fecha provisional (su escalera de repaso necesita un blanco); Ángela **no** compromete calendario real hasta su verificación de la **semana 1 de clases**, tras la cual avisa y la escalera re-apunta. Examen UNI 15-feb-2027 igual de provisional. Feriados nacionales (30-ago, 8-oct) sí firmes.

## Formatos de los mensajes (cerrados con Ángela el 2026-07-17)

Desde el 2º sync, RECONCILIACIÓN y DEMANDA van en **un solo mensaje**, en ese orden.

**DEMANDA — Richard → Ángela:**

```
Semana: <lunes DD-mmm a domingo DD-mmm>
Objetivo que manda: <semestre UNFV | UNI>
Rango de horas de Temario:
  PISO:        <N> h — por debajo se cae <hito> porque <razón>
  OBJETIVO:    <N> h
  TECHO ÚTIL:  <N> h — por encima, esta semana ya no rinde
Cesta priorizada (qué cubrir, en orden): 1) … 2) … 3) …
Proyección: a este ritmo <se llega / no se llega> a <hito> — <por qué, una línea>
Consulta: ¿hay holgura esta semana? (no supongo cómo viene tu semana)
Novedades vs lo que ya sabes: <qué cambió y por qué | "nada">
Restricciones a barrer: <preguntas sobre restricciones vigentes que ella pueda
                         resolver — ver _protocolos/capas.md | "ninguna">
```

El sync es el **barrido de restricciones**: las que dependen de Ángela (fechas, estado de MacroDroid, calendario) se preguntan aquí, porque el fallo real no es que pase la fecha, es que la condición se cumpla y nadie avise. Si una se retira, **bórrala** de la tabla de `_protocolos/capas.md`.

**RECONCILIACIÓN — Richard → Ángela:**

```
Semana cerrada: <lunes DD-mmm a domingo DD-mmm>
Temario agendado/real: <N>/<N> h
Se hizo: <temas y sesiones>
No se hizo: <bloques caídos + causa si se sabe>
Ejecución cruzada: <horas de Temario ocurridas dentro de bloques de otro frente | "ninguna">
Efecto en el plan: <vamos +/- respecto a <hito>>
Para tu MEDICIÓN: Temario <ag>/<real> | Fuente: Richard
```

La última línea es la **columna de Temario** de la línea de MEDICIÓN; Ángela ensambla la línea completa con los demás frentes (son suyos).

**ASIGNACIÓN — Ángela → Richard:**

```
Horas que caben: <N> h (del rango pedido)
Bloques: <día, hora inicio-fin> × N
Si no cabe: qué chocó + 2-3 opciones con números
Lo que veo yo de la semana real que tú no puedes ver: <…>
```

## Evento "📋 ESTADO ACTUAL" (estructura exacta, dada por Ángela)

De **día completo**, ubicado en el **LUNES de la semana en curso**. Uno por semana; los anteriores quedan de historial. Campos, **en este orden**:

| Campo | Qué lleva |
|---|---|
| `PRIORIDADES` | Orden de frentes vigente, mayor a menor. |
| `CUOTAS` | Banda semanal por frente: piso / techo. |
| `MEDICIÓN` | Una línea por semana, acumulativa (cada semana copia las anteriores). |
| `CALIENTE` | Qué domina la semana y por qué. |
| `DEADLINES` | Fechas duras próximas. Aquí entran las fechas oficiales como referencia provisional hasta que Ángela verifique la semana 1. |
| `SUEÑO` | Hora de despertar vigente + recaídas. |
| `NOTAS` | Texto libre para lo que no cabe arriba. |

Formato fijo de la línea de `MEDICIÓN`:

```
Sem DD-mmm: Frente A ag/real | Frente B ag/real | Despertar X/Y | Fuente: __
```

Los frentes son las columnas; si nace un frente, nace su columna. `ag` = agendado, `real` = ejecutado. `Fuente` marca quién reportó (Richard / Han).

**Richard toca sobre todo `MEDICIÓN`** (sus horas de Temario, reales) **y `DEADLINES`** (fechas que firme). Lo demás lo administra Ángela; lo que deba entrar y no tenga campo se manda como `NOTA` y ella lo coloca.

## Invocación (cableado en el algoritmo de arranque)

Este protocolo lo dispara el **PASO 4.5** del `README.md` raíz, en dos direcciones:

| Disparador | Dirección | Qué se hace |
|---|---|---|
| El usuario escribe `generame el prompt para Angela` (alias `sync`; se ignoran acentos y mayúsculas) | SALIDA | Generar RECONCILIACIÓN + DEMANDA con los formatos literales de arriba |
| El usuario pega un bloque cuya primera línea es `>>> ANGELA AL HABLA` | ENTRADA | Procesar su ASIGNACIÓN u otro mensaje y actualizar lo que toque |

Sin este cableado el archivo sería una regla sin invocador — el modo de falla que este proyecto ya sufrió con los cursos huérfanos. Si alguna vez se mueve o renombra este archivo, actualizar el PASO 4.5.
