# PLAN MOTOR — migración de los protocolos a código ejecutable

> **Qué es esto.** El paso a paso **ejecutable** de la Prioridad 1 de [`PENDIENTE.md`](PENDIENTE.md): convertir la contabilidad del proyecto (selector de clase, cerradura, alertas, viabilidad) en funciones Python con tests, y cablear su ejecución a mecanismos que corren **fuera del agente** (hooks de Claude Code y de git), de modo que ya no dependa de que el agente "decida" seguir un protocolo.
>
> **Quién lo ejecuta.** Un agente de Claude Code (Sonnet 5 u Opus 4.8 — ver "Qué modelo usar"), **un paso por sesión**, invocado por el usuario con el prompt literal de la sección "Cómo pedir un paso". Este documento está escrito para leerse **en frío**: no necesita ninguna conversación previa.
>
> **Invocador de este archivo:** el usuario, cuando decide dedicar una sesión a la migración; y el puntero desde la Prioridad 1 de `PENDIENTE.md`. No corre solo — es una lista de tareas, no una regla.
>
> **Por qué existe (no lo re-discutas):** el porqué completo, la evidencia (5 fallos de 5 ejecuciones el 2026-07-18) y el inventario de los 20 procedimientos viven en `PENDIENTE.md`, Prioridad 1. Este archivo solo contiene el **cómo**. Escrito el 2026-07-19 tras una revisión completa del proyecto (los 7 protocolos, README entero, datos reales de los 13 cursos).

---

## Registro de avance

Marca cada paso al cerrarlo: `[x]`, fecha y hash corto del commit. **Prohibido empezar un paso si el anterior de su fase no está marcado.**

| Paso | Nombre | Modelo | Estado |
|---|---|---|---|
| 0.1 | Esqueleto de `_motor/` + trazas + fixtures | Sonnet 5 | `[ ]` |
| 1.1 | `datos.py` — carga y validación de los JSON | Sonnet 5 | `[ ]` |
| 1.2 | `verificar.py` + alertas 17/18 + punteros huérfanos | Sonnet 5 | `[ ]` |
| 1.3 | Hook de git pre-commit | Sonnet 5 | `[ ]` |
| 1.4 | Hooks de Claude Code (SessionStart + PostToolUse) | Sonnet 5 | `[ ]` |
| 2.1 | `presupuesto.py` — integral de periodos | Sonnet 5 | `[ ]` |
| 2.2 | `alertas.py` — las 16 alertas BARATAS completas | Opus 4.8 | `[ ]` |
| 2.3 | Cableado al arranque + poda de la prosa de alertas | Opus 4.8 | `[ ]` |
| 3.1 | `grafo.py` — cerradura, mantener, urgencia | Opus 4.8 | `[ ]` |
| 3.2 | `repaso.py` — escalera, retención, olvido | Sonnet 5 | `[ ]` |
| 3.3 | `selector.py` — el PASO 5.5 completo | Opus 4.8 | `[ ]` |
| 3.4 | `viabilidad.py` — cola con prioridad + alertas CARAS | Opus 4.8 | `[ ]` |
| 3.5 | Poda de la prosa algorítmica del README | Opus 4.8 | `[ ]` |
| 3.6 | Auditoría de cierre de Fase 3 | **Fable 5** | `[ ]` |
| 4.1 | Poda de los meta-protocolos (capas, simulacro) | Opus 4.8 | `[ ]` |
| 4.2 | Decisión de porteo del motor al template | — (usuario) | `[ ]` |

**Fase 5 (backlog, sin pasos numerados todavía):** ver al final.

**Cuándo:** las fases 0–2 son baratas y protegen justo donde el proyecto se rompe (la expansión); pueden hacerse en huecos desde ya. Las fases 3–5 son las caras: después del semestre (~dic 2026) o en huecos que **no** le quiten horas al temario. **Regla del usuario (2026-07-18, sigue vigente): construir el motor en vez de estudiar es el mismo fallo con mejor tecnología.** Máximo un paso por día, y nunca en reemplazo de una sesión de estudio planificada.

---

## Cómo pedir un paso (prompt literal para el usuario)

Copia esto en una sesión nueva de Claude Code, cambiando `X.Y`:

```
Lee PLAN-MOTOR.md completo (raíz del proyecto) y ejecuta SOLO el paso X.Y,
siguiendo al pie de la letra sus "Reglas de ejecución". No hagas ningún
otro paso, aunque te sobre tiempo. Al terminar: pega la salida real de los
criterios de aceptación, marca el paso en el Registro de avance, escribe
la entrada de VERSIONES.md y dime qué commit hacer.
```

---

## Reglas de ejecución (OBLIGATORIAS para el agente que ejecute un paso)

1. **Un paso por sesión.** Ni dos pasos, ni "adelantar un poquito" del siguiente. Un paso a medias se marca como no hecho y se anota qué faltó.
2. **Pega salida real, nunca parafrasees.** Todo criterio de aceptación es un comando; su salida se pega textual en el reporte. Una salida parafraseada es indistinguible de una inventada. Si un test falla, se pega el fallo — no se reporta "casi listo".
3. **Si un test contra los datos reales NO cuadra con una cifra documentada** (p. ej. la cerradura no da 49 temas base), **NO ajustes el test para que pase ni "corrijas" el dato a ciegas**: probablemente encontraste un error real. Sigue el procedimiento de [`ERRORES.md`](ERRORES.md) ("Cómo se documenta un error nuevo") — se escribe ANTES de arreglarlo — y decide con el usuario cuál de los dos números es el verdadero.
4. **Cada paso termina con su ciclo de Capa 2 completo** (es un cambio de reglas/estructura): entrada en [`VERSIONES.md`](VERSIONES.md) con marcador `[SIM:]`, y la tabla del simulacro de la Rama 2C en el reporte. Para código, el simulacro ES la salida de los tests + la corrida contra el estado real: pégala.
5. **No toques nada fuera de lo que el paso lista** en "Archivos que toca". Si descubres algo que habría que cambiar, anótalo (bandeja de `ERRORES.md` o nota al usuario), no lo cambies.
6. **Python 3.14, SOLO biblioteca estándar.** Nada de `pip install`. `json`, `datetime`, `pathlib`, `re`, `unittest`, `argparse` bastan. Todo archivo se abre con `encoding="utf-8"` (esto es Windows; sin eso, los acentos revientan).
7. **Ninguna función lee la fecha del sistema ni rutas globales por su cuenta.** `hoy` (date) y `raiz` (Path del proyecto) se **inyectan como parámetros** en toda función pública, con default razonable solo en los puntos de entrada (`__main__`). Sin esto, los tests contra fixtures congeladas son imposibles.
8. **Toda función pública emite traza** (formato literal abajo) a stdout y a `_motor/ejecuciones.log`. Si el agente afirma haber corrido algo y el print no está en la salida, no lo corrió — ese es el mecanismo central del motor.
9. **Cuando un paso "mata prosa"**, la sección del `.md` se sustituye **en el mismo commit** por el bloque de reemplazo que el paso indica (una línea de qué hace + el nombre del módulo). Código y prosa conviviendo divergen en dos semanas — es la condición innegociable de `PENDIENTE.md`.
10. **Mensajes de error del motor en lenguaje llano**, nunca solo identificadores: "falta `modo_estudio` en 06-quimica/temario.json" sí; "error V11" no.
11. **Salida agregada, nunca una línea por elemento** cuando algo aplica a muchos (lección del patrón P8: doce líneas entierran a la que importaba).

### Especimen literal de la traza

```
[MOTOR 2026-07-19T14:03] alertas.correr_baratas(hoy=2026-07-19) -> 3 alertas: [5, 15, 18]
```

Formato: `[MOTOR <ISO con minutos>] <modulo>.<funcion>(<params clave>) -> <resumen del resultado>`. La misma línea va al log (append, nunca sobreescribir). El log está en `.gitignore`: es evidencia local de ejecución, no historia del repo.

### Estructura final de `_motor/` (se construye incrementalmente)

```
_motor/
  __init__.py
  trazas.py        # print + log append-only (paso 0.1)
  datos.py         # carga y VALIDA los JSON — única puerta de entrada (1.1)
  verificar.py     # python -m _motor.verificar [--arranque|--pre-commit] (1.2)
  hook_posttooluse.py  # valida el JSON que el agente acaba de escribir (1.4)
  presupuesto.py   # integral de periodos (2.1)
  alertas.py       # las 16 BARATAS + coste declarado en código (1.2 parcial, 2.2)
  grafo.py         # cerradura(), mantener(), urgencia_efectiva(), ciclos (3.1)
  repaso.py        # escalera, retención, predicción de olvido (3.2)
  selector.py      # PASO 5.5 completo, ramas A-F (3.3)
  viabilidad.py    # cola con prioridad + alertas CARAS + ultima_auditoria (3.4)
  tests/
    fixtures/estado-2026-07-19/   # foto congelada del estado REAL (0.1)
    test_*.py
  ejecuciones.log  # gitignored
```

---

## Qué modelo usar

- **Sonnet 5** por defecto: los pasos marcados Sonnet están especificados hasta el nivel de comando y no exigen decisiones de diseño.
- **Opus 4.8 (effort high)** en los pasos marcados Opus: portar la lógica más delicada (grafo, selector, viabilidad) y decidir qué prosa muere. Ahí una lectura superficial reintroduce los bugs que este proyecto ya pagó.
- **Fable 5 no ejecuta pasos: audita.** Su único punto en el plan es el **Paso 3.6** (auditoría de cierre de Fase 3, el punto de más riesgo). Decidido por el usuario el 2026-07-19.

---

# FASE 0 — Preparación

## Paso 0.1 — Esqueleto de `_motor/`, trazas y fixtures  `[ ]`  (Sonnet 5, ~1 sesión)

**Haz:**
1. Crea `_motor/__init__.py` (vacío) y `_motor/trazas.py` con una función `traza(modulo_funcion: str, params: dict, resultado: str, log: Path = ...)` que imprime la línea del especimen de arriba y la appendea a `_motor/ejecuciones.log`. Incluye un decorador `@trazada` opcional.
2. Añade `_motor/ejecuciones.log` a `.gitignore`.
3. Crea `_motor/tests/fixtures/estado-2026-07-19/` copiando **tal cual** (misma estructura de carpetas): `objetivo.json`, `configuracion.json`, `historial.json`, y el `temario.json` + `progreso.json` de **los 13 cursos** (`01-aritmetica` … `13-estadistica-aplicada`).
4. Crea `_motor/tests/test_fixtures.py` (unittest): todos los JSON de la fixture parsean, hay exactamente 13 cursos con temario y progreso, y la suma de temas es la real (cuéntala al copiar; el 2026-07-19 eran **160**: 12+17+13+15+17+15+9+6+12+11+11+11+11. Una versión previa de esta línea decía "150" — lo cazó el simulacro de este mismo plan: no te fíes de cifras en prosa, cuéntalas).

**Archivos que toca:** `_motor/**` (nuevo), `.gitignore`.

**Criterios de aceptación (pegar salida):**
```
python -m unittest discover -s _motor/tests -v
python -c "from _motor.trazas import traza; traza('demo.traza', {'x': 1}, 'ok')"
```
La segunda imprime la línea `[MOTOR ...] demo.traza(x=1) -> ok` y `_motor/ejecuciones.log` existe con esa línea.

**Riesgos:** ninguno especial; es plomería. No "mejores" nada de los JSON al copiarlos: la fixture es una foto, con sus defectos.

---

# FASE 1 — El verificador y los hooks (protege la EXPANSIÓN, donde el proyecto se rompe)

## Paso 1.1 — `datos.py`: carga y validación  `[ ]`  (Sonnet 5, ~1-2 sesiones)

**Haz:** `_motor/datos.py` con funciones puras que cargan y validan, devolviendo `(datos, errores: list[str])` — nunca lanzan excepción por datos malos, acumulan errores en lenguaje llano:

- `listar_cursos(raiz)` — directorios `NN-*` (dos dígitos + guion).
- `cargar_objetivo(raiz)`, `cargar_temario(raiz, curso)`, `cargar_progreso(raiz, curso)`, `cargar_historial(raiz)`, `cargar_configuracion(raiz)`.
- `validar_todo(raiz)` — corre todo lo anterior más las reglas de coherencia:
  - JSON parsea; campos obligatorios presentes (los esquemas REALES están en `_plantillas/plantilla-*.json` y en las secciones "Campos de cada tema en progreso.json" / "Valores de estado" del `README.md` — léelos de ahí, no los inventes).
  - Enums: `estado` ∈ {no_iniciado, en_progreso, completado}; `nivel_alcanzado` ∈ {null, base, intermedio, avanzado}; `nivel_repaso` ∈ {null, 1..5}; `modo_estudio.tipo` ∈ {ruta, cadencia, hito} con sus campos obligatorios por tipo (cadencia_dias entero si cadencia; condicion y condicion_urgente si hito).
  - Coherencia cruzada: todo `tema_id` de `progreso.json` existe en su `temario.json` **y viceversa**; `prerrequisitos_internos` apuntan a temas del mismo curso; `prerrequisitos_externos` con formato `"curso/tema_id"` apuntan a un tema que existe; **sin ciclos** en el grafo de prerrequisitos; todo `hito_id` de un tema existe en los `hitos` de algún objetivo que requiere ese curso; todo curso de `cursos_requeridos` existe como carpeta; fechas en ISO.
  - Deuda de esquema vieja (el README las declara muertas): si aparece `porcentaje`, `liston`, `sesiones_estimadas`, `pagado`, `ya_cobrados`, `horas_semanales` dentro de un objetivo, o `horas_semanales_disponibles` — repórtalo como error.
- `python -m _motor.datos` corre `validar_todo` sobre el repo real e imprime `OK: N cursos, M temas, 0 errores` o la lista de errores.

**Tests:** sobre la fixture. **Ojo con la regla 3 de ejecución:** si el estado real de hoy produce errores, son hallazgos reales — a `ERRORES.md` y al usuario, no silenciarlos.

**Archivos que toca:** `_motor/datos.py`, `_motor/tests/test_datos.py`.

**Criterios de aceptación:** `python -m unittest discover -s _motor/tests -v` y `python -m _motor.datos` (pegar ambas salidas).

## Paso 1.2 — `verificar.py`: el verificador único  `[ ]`  (Sonnet 5, ~1 sesión)

**Haz:**
1. `_motor/alertas.py` **mínimo**, solo con las dos alertas de higiene del repo (el resto llega en 2.2):
   - `alerta_18(raiz)` — en `VERSIONES.md`, viñetas de "Añadido"/"Cambiado" del bloque `## v` más reciente que no terminan en `[SIM: YYYY-MM-DD]` ni `[SIM: no aplica — <razón>]`. (`[SIM: no aplica]` sin razón cuenta como no puesto.)
   - `alerta_17(raiz)` — en `ERRORES.md`, sección "Bandeja de entrada", líneas que empiezan literalmente por `` - `[SIN ANALIZAR]` ``. **No contar menciones en prosa** (falso positivo real ya documentado).
2. `_motor/verificar.py`, punto de entrada `python -m _motor.verificar [--arranque | --pre-commit]`:
   - **Siempre:** `datos.validar_todo` + **punteros huérfanos**: en `README.md`, `CLAUDE.md`, `PENDIENTE.md`, `ERRORES.md`, `VERSIONES.md`, `PLAN-MOTOR.md` y `_protocolos/*.md`, extrae los enlaces markdown relativos `[..](ruta)` y las rutas con backticks que parezcan archivo del repo, y comprueba que existan. **Ignora lo que esté dentro de code spans y bloques de código** (son ejemplos, no punteros — falso positivo real detectado al simular este plan). Agrega la salida (regla 11).
   - **Siempre:** alertas 17 y 18.
   - `--arranque`: además imprime un bloque corto (≤10 líneas) pensado para inyectarse al contexto de una sesión: resultado de la validación + alertas que disparan + línea final `Verificador del motor: N comprobaciones, M hallazgos`.
   - `--pre-commit`: además, con `git diff --cached --name-only`: si el commit toca `README.md`, `_protocolos/*.md` o `_plantillas/*.json` y **no** toca `VERSIONES.md` → error (regla del ciclo de Capa 2). Exit code 1 si hay cualquier error; 0 si limpio.

**Archivos que toca:** `_motor/verificar.py`, `_motor/alertas.py`, `_motor/tests/test_verificar.py`.

**Criterios de aceptación:** tests + pegar `python -m _motor.verificar --arranque` corrido sobre el repo real. (Nota: la alerta 18 puede disparar legítimamente sobre la deuda declarada de v3 — eso es correcto, no lo "arregles".)

## Paso 1.3 — Hook de git pre-commit  `[ ]`  (Sonnet 5, ~media sesión)

**Haz:**
1. `.githooks/pre-commit` (git para Windows lo corre con su propio sh):
   ```sh
   #!/bin/sh
   python -m _motor.verificar --pre-commit || {
     echo ""
     echo "Commit bloqueado por el verificador. Detalle arriba."
     echo "Saltar A PROPOSITO (queda en el reflog): git commit --no-verify"
     exit 1
   }
   ```
2. Actívalo: `git config core.hooksPath .githooks` (es por-clon: documenta el comando en una nota corta al final de este archivo y en `PENDIENTE.md` sección del hook).
3. Pruébalo de verdad: rompe un JSON a propósito en el working tree, `git add`, intenta commitear, verifica que bloquea, restáuralo.

**Archivos que toca:** `.githooks/pre-commit` (nuevo).

**Criterios de aceptación (pegar salida):** el intento de commit bloqueado con el JSON roto, y un commit real que pasa limpio después.

## Paso 1.4 — Hooks de Claude Code  `[ ]`  (Sonnet 5, ~1 sesión)

Esto es lo que hace que el verificador corra **sin que ningún agente decida correrlo**. Los hooks los ejecuta el programa Claude Code, no el modelo.

**Haz:**
1. `_motor/hook_posttooluse.py`: lee el JSON del evento por **stdin** (trae `tool_name`, `tool_input.file_path`, …). Si `file_path` termina en `.json`, está dentro del proyecto y es de un tipo conocido (`objetivo|configuracion|historial|temario|progreso|glosario|dificultades`), lo valida con `datos.py`. Si es inválido: mensaje llano por **stderr** y `sys.exit(2)` (el 2 hace que Claude Code le devuelva el error al agente en el acto). Si es válido o no aplica: `sys.exit(0)` silencioso.
2. Crea `.claude/settings.json` (NO toques `settings.local.json`) con — especimen literal, cópialo:
   ```json
   {
     "hooks": {
       "SessionStart": [
         {
           "hooks": [
             { "type": "command", "command": "python -m _motor.verificar --arranque" }
           ]
         }
       ],
       "PostToolUse": [
         {
           "matcher": "Write|Edit",
           "hooks": [
             { "type": "command", "command": "python -m _motor.hook_posttooluse" }
           ]
         }
       ]
     }
   }
   ```
   El stdout del hook de `SessionStart` se inyecta al contexto de cada sesión nueva: por eso `--arranque` está limitado a ≤10 líneas (patrón P8: el ruido apaga la red de seguridad).
3. Prueba el hook de PostToolUse a mano: `echo {"tool_input":{"file_path":"objetivo.json"}} | python -m _motor.hook_posttooluse` (y con un JSON roto en un archivo temporal del proyecto).
4. Pide al usuario **abrir una sesión nueva** de Claude Code y confirmar que el bloque del verificador aparece al arranque. Ese es el criterio final, y no puede fingirse: o aparece o no.

**Archivos que toca:** `_motor/hook_posttooluse.py`, `.claude/settings.json` (nuevo).

**Riesgos:** si `python` no está en el PATH del proceso de Claude Code, el hook falla silencioso — verificar con la sesión nueva, no asumir. `cwd` de los hooks es la raíz del proyecto; si algo falla, usar ruta absoluta a `C:\Python314\python.exe`.

**Al cerrar este paso, la Fase 1 completa cambia el carácter del proyecto:** el arranque y la validación de datos ya no son promesas del agente. Anótalo así en `VERSIONES.md`.

---

# FASE 2 — Las alertas

## Paso 2.1 — `presupuesto.py`  `[ ]`  (Sonnet 5, ~media sesión)

**Haz:** `horas_disponibles(objetivo_json, desde: date, hasta: date) -> float` — la integral por solape de periodos de `presupuesto_horas` (la fórmula exacta está en el README, sección "El presupuesto"; periodo con `hasta: null` se extiende indefinidamente; las horas semanales se prorratean por días de solape / 7).

**Limitación a dejar impresa en la traza:** la banda semanal que negocia la agenda externa ("Ángela") **no existe como campo estructurado** — vive en prosa dentro de `_nota_viabilidad`. Mientras no exista (backlog Fase 5), la función solo integra las promesas de `presupuesto_horas`, y quien la llame debe decirlo.

**Test clave (regresión con dato real):** desde `2026-07-18` hasta `2026-09-28` sobre la fixture debe dar ≈215 h (la cifra documentada en `_nota_viabilidad` de obj-2, calculada ese día). Tolerancia ±5 h por el prorrateo de días sueltos. Si sale otra cosa fuera de eso → regla 3 de ejecución (hallazgo real, no ajustar el test).

**Archivos que toca:** `_motor/presupuesto.py`, `_motor/tests/test_presupuesto.py`.

## Paso 2.2 — `alertas.py` completo (las 16 BARATAS)  `[ ]`  (Opus 4.8, ~2 sesiones)

**Haz:** implementa en `_motor/alertas.py` las alertas **1, 2, 3, 4, 5, 6, 7, 9a, 10, 11, 13, 14, 15, 16** (17 y 18 ya están del paso 1.2). La definición operativa de cada una — la CUENTA exacta, el universo, el umbral, el texto — está en el **bloque de alertas de [`_protocolos/utilidades.md`](_protocolos/utilidades.md)**: es la fuente de verdad hasta que el paso 2.3 la pode. Léela entera antes de escribir la primera.

Reglas de implementación:
- Cada alerta es una función `alerta_N(estado, hoy) -> list[str]` con un atributo/registro `coste = "BARATA"`. Las CARAS (8, 9b, 12) **no** van aquí — llegan con `viabilidad.py` (paso 3.4); declara sus números como reservados con un comentario.
- La **alerta 15** en esta fase solo detecta y avisa que la auditoría venció (la auto-ejecución de la parte cara necesita `viabilidad.py`; hasta el 3.4, el aviso instruye correr la parte cara a mano, como hoy).
- La **alerta 6** necesita los saltos de nivel estimados (`sesiones_por_nivel` hasta el nivel requerido por los objetivos del curso): implícala con cuidado, es la más propensa a re-inventarse — usa la definición literal de utilidades.md.
- Agregación anti-ruido obligatoria (regla 11): si una alerta aplica a >3 elementos, una sola línea con la cuenta y los nombres.
- `correr_baratas(raiz, hoy) -> list[str]` es el punto de entrada que usará el selector y `--arranque`.

**Tests:** contra la fixture, con `hoy` congelado en `2026-07-19`, verifica **por nombre** qué dispara y qué no (p. ej.: la 5 debería disparar — cuenta los temas con `fecha_proximo_repaso <= 2026-07-19` en la fixture y compárala con tu implementación; la 14 debería disparar solo si `hoy >= fecha_inicio` del objetivo — con fecha_inicio 2026-08-10, NO dispara el 19-jul). Recorre los silencios: a quién deja fuera cada filtro y por qué es correcto — es la comprobación que cazó el bug de `fecha_inicio`.

**Archivos que toca:** `_motor/alertas.py`, `_motor/tests/test_alertas.py`.

## Paso 2.3 — Cableado + poda de la prosa de alertas  `[ ]`  (Opus 4.8, ~1 sesión)

**Haz:**
1. `verificar.py --arranque` pasa a incluir `correr_baratas()` (sigue con el tope de líneas: agrega).
2. **Poda** en `_protocolos/utilidades.md`: cada alerta queda reducida a UNA fila de una tabla: número, nombre, una línea de qué detecta, y **cómo se apaga** (eso es instrucción al humano/agente, no cálculo). La CUENTA, los umbrales y los textos largos **se borran**: su única casa pasa a ser `_motor/alertas.py`. Las historias de "por qué existe" que documentan errores ya están en `ERRORES.md` — verifica caso por caso antes de borrar; si alguna historia NO está en `ERRORES.md`, muévela allí primero (nunca describir lo mismo en dos sitios, pero tampoco perder historia).
3. Encabeza la tabla con: *"La definición ejecutable de cada alerta vive en `_motor/alertas.py` y corre sola en cada arranque (hook de SessionStart). Este archivo ya no la duplica."*
4. Barrido de arrastre (Rama 2B): busca referencias a "bloque de alertas de utilidades" en todo el repo (`README.md` PASO 5.5 regla 6, `capas.md`, `objetivo.json → ultima_auditoria.que_cubre`, `PENDIENTE.md`) y actualiza las que describan el mecanismo viejo.

**Criterios de aceptación:** `python -m _motor.verificar --arranque` sobre el repo real (pegar salida), y `python -m _motor.verificar` limpio de punteros huérfanos tras la poda.

---

# FASE 3 — Grafo, selector y viabilidad (el corazón; después del semestre o en huecos reales)

## Paso 3.1 — `grafo.py`  `[ ]`  (Opus 4.8, ~1-2 sesiones)

**Haz:** con las definiciones LITERALES del README (secciones "Los objetivos compiten en una cola con prioridad" y PASO 5.5 — hasta que el 3.5 las pode, son la fuente):
- `cerradura(semilla, objetivo, proyectado, estado) -> set` — corta en lo ya suficiente **para ese objetivo** (`proyectado[t] >= nivel_requerido(t, objetivo)`), recorre `prerrequisitos_internos` y `externos`, corta ciclos.
- `mantener(objetivo, estado) -> set` — el mismo árbol **sin** cortar por nivel, filtrado a temas con escalera armada (`nivel_repaso != null`).
- `urgencia_efectiva(tema, objetivos_activos) -> (prioridad, fecha_hito)` — por TEMA, no por curso: la del objetivo activo más urgente que lo necesita por tronco o por cerradura.
- `nivel_requerido(tema, objetivo)` — default del curso + `overrides`.

**Tests de regresión con los números reales documentados** (los tres salieron de bugs que costaron caro; si no cuadran, regla 3):
1. La cerradura completa de obj-2 (semestre) arrastra **49 temas base** (no 27 — el 27 fue el bug del conteo no transitivo).
2. `mantener(obj-2)` con la fixture **incluye los 7 temas ya estudiados de aritmética** (`02-numeracion` … `08-magnitudes-regla-de-tres`) y **excluye `01-conjuntos`** (solo sirve a la UNI, que está latente).
3. La cerradura del hito `parcial` (filtrando semilla por `hito_id`) da **65 temas (38 base + 27 universitarios)** — cifra de `_nota_viabilidad` del 2026-07-18.

**Archivos que toca:** `_motor/grafo.py`, `_motor/tests/test_grafo.py`.

## Paso 3.2 — `repaso.py`  `[ ]`  (Sonnet 5, ~1 sesión)

**Haz:** intervalos y retención `{1: 1, 2: 3, 3: 7, 4: 14, 5: 30}` días; `subir/bajar escalera`; `nivel_objetivo_adaptativo(tema, hitos)` = el menor nivel cuya retención cubre la distancia al hito no condicional **más lejano** que incluye el tema (tope 5); `llega_frio(tema, hito, hoy) -> bool` (retención(nivel_repaso proyectado) < días del último repaso al hito); `temas_vencidos(estado, hoy)` ordenados del más vencido al menos. Días de **calendario**, no sesiones. Tests con la fixture.

**Archivos que toca:** `_motor/repaso.py`, `_motor/tests/test_repaso.py`.

## Paso 3.3 — `selector.py`  `[ ]`  (Opus 4.8, ~1-2 sesiones)

**Haz:** el PASO 5.5 completo como `decidir_clase(raiz, hoy) -> dict`: universos `aprender()`/`mantener()` por objetivo activo, urgencia por tema, y las ramas **en orden estricto A (continuación) → B (hito urgente) → C (repaso: ≥3 vencidos o uno vencido el doble de su intervalo) → D (cadencia, con el desempate por prioridad ordinal) → E (hito normal) → F (tema nuevo: prerrequisitos internos Y externos con nivel no nulo; si uno está en null, ESE es el candidato — baja por la cadena)**. Desempates: urgencia efectiva primero, prioridad de curso después, "preguntar al usuario" como resultado explícito si persiste.

`python -m _motor.selector` imprime: las alertas baratas (PRE-FLIGHT integrado), la clase propuesta con su rama y razón en una línea, y la predicción de olvido para hitos a ≤4 semanas. **El módulo PROPONE; el que decide sigue siendo el usuario** — eso es contrato del proyecto y queda en la prosa.

**Test de regresión:** con la fixture y `hoy=2026-07-19`, calcula a mano (en el test, con los datos) qué rama debe ganar y asértalo con nombres concretos. Documenta en el test el porqué, para que un cambio de datos futuro no lo vuelva críptico.

**Archivos que toca:** `_motor/selector.py`, `_motor/tests/test_selector.py`.

## Paso 3.4 — `viabilidad.py`  `[ ]`  (Opus 4.8, ~2 sesiones)

**Haz:** la cola con prioridad del README, literal: por objetivo activo en orden de prioridad, por hito no condicional por fecha; semilla filtrada por `hito_id`; `cerradura()` del 3.1; **cobro por salto de nivel** con `proyectado[]` (nunca cobrar dos veces un curso compartido); `horas_aprender = sesiones × horas_por_sesion × 1.2`; `horas_mantener` simulando la escalera hasta el hito; `horas_por_sesion` y `minutos_por_repaso` **medidos** de `historial.json` (≥5 sesiones / ≥3 repasos) o provisionales 1.5 h / 12 min **declarando que son supuestos** en la salida. CABE/NO CABE contra `presupuesto.horas_disponibles` menos lo comprometido. Con esto: **alertas 8, 9b y 12** (las CARAS), la **auto-ejecución** de la parte cara cuando la alerta 15 dispara, y el **escritor de `objetivo.json → ultima_auditoria`** (pasa a ser este módulo su único escritor; actualizar la nota del campo).

**Test de regresión:** con la fixture, `hoy=2026-07-18` y los provisionales 1.5 h/12 min: déficit del parcial ≈44 h y del final ≈62 h acumuladas (cifras de `_nota_viabilidad`). Tolerancia ±10% — el modelo de mantenimiento asume cierre uniforme y es aproximación declarada. Fuera de eso: regla 3.

**Archivos que toca:** `_motor/viabilidad.py`, `_motor/tests/test_viabilidad.py`, nota de `ultima_auditoria` en `objetivo.json`.

## Paso 3.5 — Matar la prosa algorítmica del README  `[ ]`  (Opus 4.8, ~1-2 sesiones — EL PASO MÁS DELICADO)

**Haz** (con `VERSIONES.md` y `ERRORES.md` releídos antes — Rama 2B entera):
1. **PASO 5.5:** el cuerpo algorítmico (universos, ramas, desempates, pre-flight) se sustituye por: qué decide el paso en 3 líneas, la orden `corre python -m _motor.selector y pega su salida`, y las reglas que son **contrato humano, no cálculo** (propones-no-impones; no hardcodear cursos; la nota de segunda sesión del día; no inventar tipos de clase). Los "POR QUÉ" históricos largos → verificar que estén en `ERRORES.md` y borrar de aquí.
2. **"Los objetivos compiten en una cola con prioridad"** (pseudocódigo de cerradura y cola): → 5 líneas de concepto + puntero a `grafo.py`/`viabilidad.py`.
3. **Escalera de repaso y retención:** las TABLAS de intervalos se quedan (las lee el humano); la mecánica de subir/bajar/adaptativo → puntero a `repaso.py`.
4. **Integral del presupuesto** → puntero a `presupuesto.py`.
5. Barrido de arrastre TOTAL: `grep` de "PASO 5.5", "cerradura", "cola con prioridad", "mantener(" sobre TODO el repo (protocolos, `objetivo.json`, `PENDIENTE.md`, `ERRORES.md`) y actualizar cada puntero al nuevo destino. `python -m _motor.verificar` limpio al final.
6. El **aviso de LÍMITES REALES** del inicio del README se actualiza: el punto 1 ("nada obliga al agente") se matiza — el arranque, la validación de JSON y las alertas ya corren fuera del agente; lo que sigue siendo prosa (juicio pedagógico, protocolos de cierre) sigue bajo el aviso. **No borres el aviso**: acótalo a lo que sigue siendo verdad.

**Criterio de aceptación:** además del verificador limpio, una prueba de sesión fría: abrir una sesión nueva y pedir "¿qué clase toca hoy?" — debe correr el selector y pegar su salida, no improvisar el cálculo.

---

## Paso 3.6 — Auditoría de cierre de Fase 3  `[ ]`  (Fable 5, ~1 sesión — decidido por el usuario el 2026-07-19)

**Quién y cuándo:** la ejecuta **Fable 5**, en una sesión propia, cuando los pasos 3.1–3.5 estén marcados. Es el único punto del plan reservado para Fable: la Fase 3 concentra el riesgo (la lógica más delicada + la poda más grande) y el usuario prefiere gastar aquí sus tokens de Fable en vez de en la ejecución. El prompt del usuario puede ser tan corto como *"toca la auditoría de Fase 3 del plan del motor"* — este apartado es todo el contexto que la sesión necesita.

**Qué NO es:** releer el código y opinar que se ve bien. La lección fundacional de este proyecto (aviso del README, 2026-07-18) es que la revisión por lectura no caza esta clase de errores. Todo veredicto de esta auditoría se sostiene en una **ejecución pegada** o en una **re-derivación independiente**, nunca en "lo revisé".

**Qué hace, en orden:**
1. **Re-derivación independiente:** recalcular desde los JSON crudos (sin usar `_motor/`) al menos: el tamaño de la cerradura del objetivo prioritario, el conjunto `mantener()`, la clase que el selector debería proponer hoy, y el CABE/NO CABE del hito más cercano. Comparar contra la salida de los módulos. Toda divergencia es un hallazgo.
2. **Revisar los tests como sospechosos, no como evidencia:** ¿los tests de regresión siguen anclados a los números documentados o alguien los "ajustó para que pasen" (violando la regla 3 de ejecución)? Revisar el historial de git de `_motor/tests/` buscando cambios de cifras esperadas.
3. **Verificar que la prosa murió de verdad:** grep de los algoritmos podados (ramas del selector, pseudocódigo de la cola, CUENTA de alertas) sobre TODO el repo. Si una descripción algorítmica sobrevive en un `.md` además del código, es la divergencia anunciada — hallazgo.
4. **Prueba de sesión fría real:** abrir una sesión nueva de Claude Code (el usuario la abre; Fable dicta qué pedir): ¿el bloque del verificador aparece inyectado al arranque? ¿Ante "¿qué clase toca hoy?" el agente corre `python -m _motor.selector` y pega su salida, o improvisa el cálculo en prosa? Lo segundo es fallo de cableado aunque el número salga igual.
5. **Barrido de dependencias silenciosas** (el patrón de "quién más leía esto"): por cada sección podada en 3.5 y 2.3, ¿qué archivos apuntaban a ella y adónde apuntan ahora? `python -m _motor.verificar` limpio es necesario pero no suficiente — los punteros semánticos ("ver la sección X") no los caza un script.
6. **Cierre:** cada hallazgo se documenta en `ERRORES.md` ANTES de arreglarse (procedimiento de ese archivo); el reporte final lista hallazgos con severidad y qué paso los introdujo; la entrada de `VERSIONES.md` de esta auditoría enlaza a los patrones tocados. Si no hay hallazgos, decirlo explícitamente con la evidencia de cada comprobación — "sin hallazgos" es un resultado; no decir nada no lo es.

---

# FASE 4 — Poda de los meta-protocolos

## Paso 4.1 — `capas.md`, Rama 2C y CLAUDE.md  `[ ]`  (Opus 4.8, ~1 sesión)

Gran parte del andamiaje meta (simulacros, puertas de salida, marcadores) existe para compensar que nada verificaba nada. Con el motor y los hooks vivos:
- **Rama 2C** para cambios **del motor**: el simulacro ES `unittest` + la corrida real pegada — dilo explícitamente para no exigir la tabla manual redundante donde el código ya deja rastro. Para cambios de **prosa/reglas**, la Rama 2C sigue igual.
- **Alertas 17/18** ya corren solas por hook (no por buena voluntad del arranque): actualizar sus menciones.
- **`CLAUDE.md`**: el paso 0 del arranque ya no necesita pedir fe — el hook inyecta el estado. Reescribir el aviso para que dirija la atención del agente a lo que sigue siendo prosa.
- **NO tocar:** el ciclo de `VERSIONES.md`/`ERRORES.md` (sigue siendo la memoria del proyecto), ni la regla 6 (procedimiento canónico), ni nada del flujo de estudio.

## Paso 4.2 — Porteo del motor al template  `[ ]`  (decisión del usuario)

`_porteo-template.md` porta la arquitectura v2/v3 al template genérico. Decidir con el usuario: ¿el template hereda el motor (y el porteo se actualiza), o se porta solo la arquitectura de datos y cada derivado trae su motor después? Sin decisión, no hacer nada aquí.

---

# FASE 5 — Backlog (sin fecha; cada item exige su propio mini-plan al activarse)

- **`cierre.py`** — escritura de `progreso.json`/`historial.json` al cerrar clase: el agente decide el nivel (juicio), la función valida y escribe (mecánica) y se niega a avanzar si nadie decidió. Es el patrón "el juicio se queda, su registro se codifica" del inventario de `PENDIENTE.md`.
- **Banda semanal estructurada** — campo `banda_semanal_vigente` en `objetivo.json` (piso/techo/desde/hasta), escrito por el protocolo de sync con la agenda; `presupuesto.py` y `viabilidad.py` pasan a preferirla sobre las promesas. Hoy la banda vive en prosa y "si hay banda vigente, manda la banda" no es computable.
- **`backup.py`** — trivial; solo si el comando manual da problemas.
- **Auditoría técnica periódica** — la idea sin desarrollar de `PENDIENTE.md`: checkpoint fechado + alerta barata, corriendo `verificar` + comprobaciones estructurales nuevas. El molde es la alerta 15.
- **Generador de formatos de sync** (RECONCILIACIÓN/DEMANDA) — plantillas rellenadas con datos del motor.

---

## Qué NO se codifica nunca (para que nadie lo intente)

El 30% de juicio del inventario de `PENDIENTE.md` (items 15–20): decidir si el usuario cumplió un `criterio_dominio`, redactarlo, explicar temas, generar ejercicios, la entrevista, decidir qué recortar. **Ahí es donde el agente aporta**; el motor solo registra que el juicio se emitió.

## Notas de instalación por-clon (si el repo se clona en otra máquina)

```
git config core.hooksPath .githooks
```
Los hooks de Claude Code viajan solos en `.claude/settings.json`. Python ≥3.10 requerido en PATH.
