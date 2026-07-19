# PENDIENTE — Migración al modelo de dos dimensiones

> **Estado (2026-07-17):** la migración interna está **CERRADA** y el `README.md` es la única fuente de verdad viva. Queda **una sola cosa**, y ocurre fuera de este repo. Cuando se cierre: borra este archivo y quita el aviso de migración de `CLAUDE.md`.

---

## Lo que queda

---

# 🔴 PRIORIDAD 1 — MIGRAR LOS PROTOCOLOS A FUNCIONES REALES

> **Decidido por el usuario el 2026-07-18.** Se ejecutará **después del semestre** (~diciembre 2026) o cuando haya hueco. **Muy probablemente se desarrolle con otro agente (Fable), no con Claude Code** — por las limitaciones que se documentan más abajo, que son la razón de existir de este apartado.
>
> **ACTUALIZACIÓN 2026-07-19 — ya existe el paso a paso ejecutable: [`PLAN-MOTOR.md`](PLAN-MOTOR.md).** Lo generó Fable tras una revisión completa del proyecto; la ejecución de cada paso será con Sonnet 5 / Opus 4.8 siguiendo ese plan, un paso por sesión. Dos matices que el plan incorpora sobre lo escrito aquí: (1) además del hook de git, existen **hooks de Claude Code** (SessionStart, PostToolUse) que corren en la máquina, fuera del agente, en cada sesión — son el enforcement primario, no solo el commit; (2) el "después del semestre" se acotó: las **Fases 0–2** del plan (verificador + hooks + alertas) son baratas, protegen la expansión —donde el proyecto se rompe— y pueden adelantarse en huecos; las Fases 3–5 (grafo, selector, viabilidad, poda) siguen para después del semestre. **Este apartado conserva el PORQUÉ y el inventario; el CÓMO vive solo en el plan** — no los dupliques.
>
> **Este documento está escrito para leerse en frío, sin nada del contexto de la sesión que lo originó.** Si eres un agente que llega aquí sin haber vivido el 18-jul-2026, todo lo que necesitas está en este texto. No hay que reconstruir nada de ninguna conversación.

## Por qué. La evidencia, no la opinión

El 2026-07-18 se dedicó **una sesión entera** a auditar el proyecto. Resultado: **cinco fallos estructurales, los cinco cazados por el usuario o por un script — ninguno por los mecanismos del proyecto**, que llevaban semanas escritos y en teoría vigentes.

| Fallo real | Cuánto llevaba escondido | Cómo se encontró |
|---|---|---|
| La cerradura del semestre contaba **27 temas base cuando son 49** (se contaba solo el primer salto, no la cadena transitiva). Toda la viabilidad del parcial estaba mal | desde el 17-jul | ejecutando la cerradura |
| **7 temas ya estudiados** de los que depende el semestre se caían del universo de repaso: 12 sesiones de trabajo desvaneciéndose en silencio | introducido ese mismo día, al arreglar otra cosa | ejecutando el conjunto |
| Un filtro **silenciaba los 5 cursos del objetivo prioritario** — lo contrario de lo que la comprobación existía para hacer | horas | el usuario, razonando |
| Una comprobación producía **12 líneas por sesión**, suficiente para que se dejara de leer el bloque entero | horas | ejecutando el arranque |
| Otra **se disparaba sobre su propia documentación** (contaba las menciones en prosa de su propia marca) | horas | ejecutando la comprobación |
| Un refactor propuesto como "barato" resultó tocar **202 puntos**, 26 de ellos en archivos de datos | — | contándolos |

**Cinco de cinco veces que se ejecutó algo, apareció un fallo. Leyendo no apareció ninguno.**

## Las tres limitaciones del agente que hacen inevitable esta migración

Documentadas también al principio del `README.md`. Son la causa raíz, no una queja:

1. **Nada obliga al agente a seguir los protocolos.** Son texto en su contexto. `CLAUDE.md` se carga solo, pero cargarse no es ejecutarse. El 18-jul el agente incumplió cinco reglas **teniéndolas leídas**.
2. **Un archivo de rastro demuestra que un paso ocurrió, no que se hiciera bien.** Los marcadores los escribe el propio agente; puede escribir cualquier cosa. Un script verifica que la tabla existe, no que sea seria.
3. **Escribir una regla no cambia el comportamiento.** Demostrado dos veces con el mismo problema (citar identificadores internos): la regla existía, el agente la incumplió en cada mensaje posterior, incluido aquel en el que la concedía.

**Conclusión operativa:** más documentación no arregla esto. Es el método que falló. Lo único que funcionó fue ejecutar código.

## EL MECANISMO CLAVE — idea del usuario, y es la mejor de la sesión

> **Cada función imprime al ejecutarse.** Si el agente afirma haber corrido algo y **el print no está en la salida, no lo corrió**. Se ve en un vistazo.

Esto resuelve el problema que ninguna regla escrita pudo resolver, y por una razón concreta: **el agente ya no puede afirmar sin poder ser contrastado.** Hoy, cuando el agente dice "toca porcentajes", la única opción del usuario es creerle. Con `selector_clase()` existiendo, el usuario corre la función y compara. La honestidad del agente deja de ser el eslabón débil.

**Requisitos del rastro de ejecución (no negociables al diseñar):**
- Cada función pública imprime una línea identificable al entrar: qué función, con qué parámetros clave, y qué devolvió.
- El rastro va a **stdout y a un log con fecha** (`_motor/ejecuciones.log`), para que se pueda auditar después, no solo en el momento.
- **El agente debe pegar la salida real, nunca parafrasearla.** Una salida parafraseada es indistinguible de una inventada.
- El log es **append-only**, igual que `historial.json`.

## Inventario COMPLETO de procedimientos, clasificado

Auditado el 2026-07-18 recorriendo los protocolos uno a uno. **20 procedimientos: 14 son algoritmo puro (70%), 6 son juicio real (30%).**

### CODIFICABLE — el 70%. Todo esto debe convertirse en funciones con tests

| # | Procedimiento | Dónde está hoy | Qué es en realidad |
|---|---|---|---|
| 1 | **Selector de clase** (ramas A continuación, B hito urgente, C repaso, D cadencia, E hito normal, F tema nuevo) | `README.md`, PASO 5.5 | Lee JSON, evalúa condiciones en orden fijo, devuelve un tipo de clase y un tema |
| 2 | **Cerradura transitiva `aprender()`** | `README.md` | Recorrido de grafo con corte en lo que ya está a nivel suficiente |
| 3 | **Conjunto `mantener()`** | `README.md` | El mismo grafo **sin** corte por nivel — es lo que sostiene el repaso |
| 4 | **Urgencia efectiva por TEMA** | `README.md` | Mínimo de `prioridad` sobre los objetivos activos que lo requieren, por tronco o por cerradura |
| 5 | **Cola de viabilidad por hito** | `README.md` | `horas_aprender` (saltos de nivel × h/sesión × 1.2) + `horas_mantener` (repasos × min/repaso) |
| 6 | **Integral de `presupuesto_horas`** | `README.md` | Solape de cada periodo con `[hoy, fecha]` × horas semanales |
| 7 | **Escalera de repaso** (subir, bajar, recalcular fechas, tope adaptativo al hito) | `README.md` | Aritmética de fechas con tabla de intervalos 1/3/7/14/30 |
| 8 | **Predicción de olvido** | `README.md` | `retención(nivel_repaso)` contra días hasta el hito |
| 9 | **Las 18 comprobaciones** del bloque de alertas | `_protocolos/utilidades.md` | Booleanos sobre JSON; las caras (8, 9b, 12) usan la cola de viabilidad |
| 10 | **Despertar objetivos latentes** | `README.md`, PASO 2a | Evaluar `condicion_activacion` (ya exigida como aritmética verificable) |
| 11 | **Validación estructural de temarios** | `README.md`, reglas de calidad | Detectar ciclos, prerrequisitos declarados a nivel de curso, campos ausentes |
| 12 | **Escritura de `progreso.json` e `historial.json` al cerrar clase** | `README.md` | Mecánico **una vez decidido el nivel** (la decisión es juicio, la escritura no) |
| 13 | **Backup** | `_protocolos/utilidades.md` | Comprimir directorio |
| 14 | **Formatos de sync con la agenda** (reconciliación, demanda) | `_protocolos/coordinacion-agenda.md` | Rellenar plantilla fija con datos calculados |

### JUICIO REAL — el 30%. Se queda en prosa, y ahí es donde el agente aporta

| # | Procedimiento | Por qué no se codifica | Cómo tratarlo |
|---|---|---|---|
| 15 | **Decidir si el usuario cumplió el `criterio_dominio`** | Juicio sobre respuestas en texto libre | El agente decide; la **función registra** la decisión y quién la tomó |
| 16 | **Redactar el `criterio_dominio` de un nivel** | Juicio pedagógico | Prosa. La función puede **exigir** que exista antes de dejar subir el nivel |
| 17 | **Explicar el tema, generar ejercicios, Feynman** | Es el producto real del proyecto | Prosa pura. Aquí es donde el agente debe estar, no en contabilidad |
| 18 | **Entrevista de setup inicial** | Conversación abierta | Prosa. La función **valida** que el resultado esté completo |
| 19 | **Decidir QUÉ recortar cuando no cabe** | Decisión del usuario, no del agente | **Las opciones SÍ se calculan**: la función presenta alternativas con números; el usuario elige |
| 20 | **Checklist de Capa 2** (invocador, arrastre, contradicciones) | Exige leer prosa y entender semántica | Parcialmente automatizable con un **registro de reglas** donde cada regla declare su invocador en un campo, no en un párrafo |

**El patrón que se repite en los seis:** el juicio se queda, **la verificación de que el juicio se emitió se codifica**. La función no decide si aprendiste; se niega a avanzar si nadie lo decidió.

## Arquitectura propuesta

```
_motor/
  datos.py        # cargar y VALIDAR los JSON (esquema + coherencia), única puerta de entrada
  grafo.py        # cerradura(), mantener(), urgencia_efectiva(), detección de ciclos
  presupuesto.py  # integral de periodos, horas disponibles hasta una fecha
  repaso.py       # escalera, próximas fechas, predicción de olvido
  viabilidad.py   # cola con prioridad por objetivo y por hito
  selector.py     # PASO 5.5 completo, ramas A-F
  alertas.py      # las 18 comprobaciones, con su coste declarado EN CÓDIGO
  cierre.py       # escritura de progreso/historial al cerrar clase
  trazas.py       # el print obligatorio + log append-only
  tests/
    fixtures/     # FOTOS CONGELADAS del estado del proyecto (real y sintéticas)
    test_*.py
```

**Decisiones de diseño ya tomadas, para no rediscutirlas:**
- **Python.** Ya está en la máquina y es lo que se usó para todas las verificaciones del 18-jul.
- **Los tests usan fotos congeladas del estado real**, no datos inventados: los fallos aparecieron con los datos reales, no con ejemplos de manual.
- **Cada función pública imprime su traza** (ver el mecanismo clave, arriba).
- **El coste de cada comprobación (barata/cara) se declara en el código**, no en una tabla de un `.md` que hay que mantener a mano.
- **Un solo verificador, varios disparadores:** un `python -m _motor.verificar` que se pueda lanzar desde el hook de git, desde el arranque de sesión, y a mano. No tres implementaciones.

## LA CONDICIÓN INNEGOCIABLE: la prosa tiene que MORIR, no convivir

**Si el `README.md` sigue describiendo el selector de clase Y además existe `selector.py`, divergen en dos semanas y el proyecto vuelve exactamente a donde está hoy.** Es el fallo que este repo ya cometió con las cifras (una nota decía 27 temas cuando la cerradura daba 49).

Regla de migración: **cuando un procedimiento pasa a función, su descripción algorítmica se BORRA del `.md` y se sustituye por (a) una línea de qué hace y (b) el nombre de la función.** El código es la verdad. La prosa es el índice.

## Riesgo que hay que tener presente al ejecutar esto

**Este proyecto existe para aprobar un semestre, no para ser un buen software.** El 18-jul se gastó la sesión entera en arquitectura mientras el parcial acumulaba un déficit de ~44 h. Construir el motor en vez de estudiar es el mismo fallo con mejor tecnología.

**Por eso: se hace después del semestre, o en huecos que no le quiten horas al temario.** Y si hay que priorizar dentro del motor, el orden por valor es **1, 2, 3, 4, 9, 5** (selector, grafo, urgencia, comprobaciones, viabilidad) — ahí caen los seis fallos del 18-jul.

---

### HOOK DE GIT — el primer control que vive FUERA del agente · pedido por el usuario 2026-07-18

> **Nota (2026-07-18, tras el estudio):** el hook por sí solo caza **4 de los 6** fallos de ese día, y **solo se dispara al commitear** — si pasan días sin commit, no detecta nada. Lo valioso no es el hook: es **el verificador**, que debe poder lanzarse desde el hook, desde el arranque de sesión y a mano. Ver la arquitectura de la prioridad 1: este apartado es **un disparador de aquello**, no una herramienta aparte.

**Por qué es distinto a todo lo demás que hay en este proyecto.** Todos los controles actuales corren **dentro** del agente: si una sesión no ejecuta el algoritmo de arranque, no se dispara ninguno. Un hook de git corre en la máquina, en el momento del commit, **sin importar qué agente, qué sesión, ni si alguien se acordó de algo**. Es el único mecanismo disponible hoy que puede decirse honestamente al 100%.

**Qué debe bloquear el commit** (arrancar por aquí; todo esto es verificable con scripts, sin juicio):
1. **Apuntes de versión sin marcar como probados** — viñetas de la versión más reciente de `VERSIONES.md` sin su marcador `[SIM:]`, o con `[SIM: no aplica]` sin razón escrita.
2. **Se tocó una regla y no se escribió su versión** — si el commit modifica `README.md`, `_protocolos/*.md` o un esquema de datos y **no** toca `VERSIONES.md`, falta el registro.
3. **JSON inválido** en cualquier `objetivo.json`, `temario.json`, `progreso.json`, `configuracion.json`, `historial.json`.
4. **Punteros huérfanos** — enlaces a archivos o secciones que no existen.
5. **Cifras en prosa que contradicen lo calculable** — el caso real: una nota decía "27 temas base" cuando la cerradura da 49. Comparar los números afirmados contra los computados.
6. **Comprobaciones sin coste declarado** en la tabla de baratas/caras de `_protocolos/utilidades.md`.

**Diseño, para no repetir errores ya conocidos:**
- **Que se pueda saltar a propósito y quede constancia** (`--no-verify` deja rastro en el reflog). Un control que no se puede desactivar en una emergencia acaba desinstalado entero.
- **Mensajes de error en lenguaje llano, no en identificadores.** Si el hook dice "falla la comprobación 18", el usuario no sabrá qué hacer. Debe decir qué falta y dónde.
- **Rápido.** Un hook lento se desactiva a la semana.
- **Escrito en Python** (ya está disponible en la máquina y es lo que se ha usado para todas las verificaciones de este proyecto).

**Estado: PLANIFICADO.** El usuario lo pidió expresamente y aprobó su prioridad. Es el **Paso 1.3 de [`PLAN-MOTOR.md`](PLAN-MOTOR.md)** (y los hooks de Claude Code, que este apartado no contemplaba, son su Paso 1.4).

---

### AUDITORÍA TÉCNICA del proyecto — idea del usuario, 2026-07-18, SIN DESARROLLAR

**El problema que resuelve.** Hoy existe una sola cosa llamada "auditoría" y **no audita el proyecto: audita el plan de estudios.** La corrida cara de `estado` recorre objetivos por prioridad e hitos por fecha para responder *"¿llego a mis metas?"*. Es útil y no es esto.

Lo que falta es una **auditoría TÉCNICA**: revisar que el proyecto **como sistema** sea coherente. Contradicciones entre archivos, punteros huérfanos, reglas sin invocador, campos sin escritor, protocolos que se contradicen, cosas que deberían dispararse al arrancar una sesión y no se disparan.

**Por qué importa (y por qué la idea nació el 2026-07-18).** Ese día, en una sola sesión, salieron **cuatro errores estructurales**, tres de ellos cazados por el usuario y no por el proyecto: una alerta bien escrita que no podía dispararse nunca, un campo usado como condición que significaba cosas distintas en dos objetivos, un arreglo que reintrodujo un bug viejo por otra capa, y otro arreglo que huerfanaba 12 sesiones de trabajo ya hecho. **Ninguno lo detectó ningún mecanismo del proyecto.** Los detectores existentes miran los DATOS del plan; nadie mira la ESTRUCTURA.

**Forma prevista.** Periódica y auto-disparada, con el mismo patrón que ya funciona para la auditoría cara: un checkpoint fechado + una alerta barata que vigila su vencimiento y **la corre sola**, no que la recuerde. Ver la alerta 15 de `_protocolos/utilidades.md` como molde — el mecanismo ya está probado, lo que falta es el contenido de las comprobaciones.

**Insumos que ya existen y hay que reutilizar, no reinventar** (regla irrompible 6):
- Los **10 patrones de [`ERRORES.md`](ERRORES.md)** son literalmente el catálogo de qué buscar. Varios son verificables por máquina o casi (punteros huérfanos, campos sin escritor, alertas sin coste declarado).
- El **checklist de Capa 2** de `_protocolos/capas.md` ya enumera las preguntas correctas; la auditoría sería correrlas sobre lo que YA está escrito, no sobre lo que se va a escribir.
- La **Rama 2C (simulacro)** es la técnica: ejecutar contra el estado real en vez de leer.

**Cuidado al diseñarla** — riesgos ya conocidos, no los redescubras:
- **Coste.** Si es cara, clasifícala como CARA y dale checkpoint fechado. No la cuelgues del arranque de cada sesión.
- **Ruido.** Si escupe treinta hallazgos, se deja de leer y se apaga sola (patrón P8). Agregar y priorizar desde el principio.
- **No la conviertas en juicio.** El README exige aritmética verificable. Lo que no se pueda comprobar mecánicamente, que quede como pregunta al usuario, no como veredicto del agente.
- **No robe horas al objetivo prioritario.** Con el semestre en déficit, una auditoría técnica larga es tiempo que sale del parcial. Barata y periódica > exhaustiva y rara.

**SU PRIMER TRABAJO, YA IDENTIFICADO: los simulacros retroactivos.** El simulacro obligatorio (comprobar una regla ejecutándola contra el estado real antes de darla por hecha) se inventó el 2026-07-18. **Todo lo escrito antes —o sea, casi todo el proyecto, de la v1 a la v3— nunca se ejecutó contra datos reales: se escribió, se leyó, y se dio por bueno.** Cada vez que en esta sesión se ejecutó algo que llevaba semanas escrito, salió un fallo: la cerradura contaba 27 temas donde había 49, una comprobación silenciaba los cursos del objetivo prioritario, otra se disparaba sobre su propia documentación. **No hay razón para pensar que el resto esté mejor.** La auditoría debe ir regla por regla ejecutándolas, empezando por las que deciden horas y fechas.

**Y el problema de fondo que esto destapa: las decisiones arbitrarias acumuladas.** El usuario lo señaló el 2026-07-18: *"¿cuántas decisiones arbitrarias habrás tomado en sesiones anteriores que ya olvidaste, y qué errores salieron de ahí?"* Es la pregunta correcta y nadie la puede contestar hoy — precisamente porque no había registro. Desde ahora `VERSIONES.md` y `ERRORES.md` lo dejan por escrito, pero **todo lo anterior a v3 es opaco**: hay criterios metidos en los archivos sin que conste quién los decidió, por qué, ni contra qué alternativa. La auditoría técnica es la vía para sacarlos a la luz uno a uno y validarlos o tirarlos.

**Estado: NO EMPEZADA.** Decisión explícita del usuario: dejarla anotada, no desarrollarla ahora. Recogida como item del backlog (Fase 5) de [`PLAN-MOTOR.md`](PLAN-MOTOR.md).

---

### Portar la arquitectura al TEMPLATE de origen — fuera de este repo

Está listo el prompt autocontenido en [`_porteo-template.md`](_porteo-template.md): se copia en una sesión abierta en el repo del template. El porteo en sí ocurre allá, no aquí. Cuando esté hecho, borra ese archivo y este.

---

## Lo que se cerró

Los **7 bloques de deuda de la migración** (2026-07-16): `porcentaje` eliminado de los `progreso.json`, el escritor que faltaba de `nivel_alcanzado`, las contradicciones del README resueltas, los protocolos migrados al lenguaje de niveles, las rúbricas de nivel por curso, la prosa hecha funcional (escalera adaptativa, predicción de olvido, coste de mantenimiento) y la auditoría punta a punta de "ningún campo sin escritor, ninguna regla sin invocador". Todo commiteado; el detalle vive en el historial de git, no aquí.

### La conversación de las horas — cerrada el 2026-07-17

Era el objetivo final de la migración y se dejó para el final a propósito, porque dependía de tener el modelo honesto cerrado. **Ya no es deuda:** produjo un mecanismo vivo, y cada parte de su contenido tiene casa propia.

| Qué | Dónde vive ahora |
|---|---|
| El mecanismo de coordinación con la agenda externa (loop dominical, formatos, reparto de roles, marcas de autoría) | [`_protocolos/coordinacion-agenda.md`](_protocolos/coordinacion-agenda.md), invocado por el **PASO 4.5** del `README.md` |
| Las horas realmente disponibles, semana a semana | Las negocia Ángela en cada sync con una banda (piso/techo). El `presupuesto_horas` de `objetivo.json` sigue siendo **la promesa** a contrastar, no la medición |
| El déficit vigente y los supuestos sobre los que descansa | `_nota_viabilidad` en `objetivo.json` |
| Que esos números son supuestos mientras no se midan | Regla 10 del `README.md` + alertas 10 y 11 del comando `estado` |
| Cuándo se decide recortar el temario | En **cada sync**, con el checkpoint del **26-jul-2026** como primera medición real. Ver "Ajuste del plan" en el protocolo de coordinación |
| Que los 5 temarios del semestre son inferidos y hay que validarlos contra el sílabo real | **Alerta 14** del comando `estado` (antes era prosa sin invocador) |

**La decisión estructural —qué se recorta del temario— NO está tomada.** Está *fechada* y con *mecanismo*: se revisa en cada sync y la primera medición real llega el 26-jul. Eso es distinto de estar pendiente, y es la diferencia que este archivo tenía que dejar de confundir.
