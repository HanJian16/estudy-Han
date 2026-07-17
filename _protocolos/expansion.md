# Protocolo de ampliación de plan (nuevos objetivos)

> Este archivo lo lee el agente cuando el usuario quiere **agregar un nuevo objetivo** a un plan que ya existe (típicamente después de completar el objetivo anterior, o cuando quiere expandir horizontes en paralelo).

---

## Cuándo aplicar este protocolo

Aplica este protocolo cuando:

1. **El usuario lo pide explícitamente**: "quiero aprender X además", "ya terminé mi objetivo, ¿ahora qué?", "quiero seguir con cálculo 2", etc.
2. **El agente detecta que todo el plan actual está al 100%**: al leer todos los `progreso.json`, todos los temas están en `"completado"`. En ese caso, felicita al usuario y ofrece proactivamente ampliar el plan.

**NO aplicar** si el usuario solo quiere ajustar el plan actual (recortar temas, cambiar deadline, etc.) — eso es re-planificación, no ampliación.

---

## Principio fundamental: conservar el conocimiento

Un nuevo objetivo **NO borra el pasado**. Todo lo que el usuario ya aprendió debe conservarse y aprovecharse:

- Los cursos ya generados **se mantienen** (no se borran carpetas).
- Los `progreso.json` de cursos completados **se mantienen** (son evidencia del conocimiento).
- Los `glosario.json` y `dificultades.json` **se conservan y crecen** — el vocabulario y las dificultades históricas son datos valiosos.
- Los temas ya completados en cursos existentes **se cuentan como prerrequisitos ya cumplidos**.

---

## Paso 1: Entrevista de ampliación

Haz las siguientes preguntas al usuario, una por una:

1. **Nuevo objetivo**
   "¿Qué nuevo objetivo quieres perseguir?"

2. **Relación con lo anterior**
   "¿Este objetivo continúa lo que ya aprendiste (ej: cálculo 2 después de cálculo 1) o es un área nueva?"

3. **Fecha límite**
   "¿Hay deadline para este objetivo? ¿Para cuándo?"
   *(Si no hay, `fecha_limite: null` y no se le aplican validaciones de tiempo — pero sus horas sí cuentan contra el presupuesto.)*

4. **Estado de los objetivos anteriores**
   "¿Damos por completado algún objetivo anterior, o quieres mantenerlos todos activos en paralelo?"
   - Si dice completado → pon `estado: "completado"` y `fecha_completado` en ese objetivo. Se queda en el array `objetivos`: su historia es evidencia del conocimiento. Deja de competir por horas automáticamente.
   - Si dice en paralelo → simplemente agrega el nuevo al array `objetivos` con `estado: "activo"`. Es una situación soportada y normal: objetivos distintos con ciclos de vida distintos (ver "Objetivos paralelos" en el `README.md` raíz).

5. **¿Es un objetivo para AHORA o para después?**
   "¿Esto lo empiezas ya, o solo tiene sentido cuando termines otra cosa?"
   - Ahora → `estado: "activo"`, `condicion_activacion: null`.
   - Después → `estado: "latente"` y escribe su `condicion_activacion` en **aritmética verificable** (ej: `"cuando obj-1 pase a estado completado"`). El objetivo se genera entero —temario incluido— pero no compite por horas hasta que despierte. El PASO 2a del `README.md` raíz lo despierta solo.
   Un objetivo latente es la forma correcta de registrar "esto lo haré después del examen" sin que contamine la planificación de ahora ni se olvide.

6. **Hitos**
   "¿Qué fechas de corte tiene? ¿Hay más de una, y cubren cosas distintas?"
   Llena el array `hitos`. Si solo hay una fecha, un hito basta. Si hay varias que cubren contenido distinto (parcial/final de un semestre), decláralas todas y marca con `condicional: true` las que solo ocurren si algo sale mal (sustitutorios). Ver "Hitos" en el `README.md` raíz.

7. **Prioridad relativa**
   "Si algún día tienes que elegir entre estos objetivos, ¿cuál manda?"
   Asigna `prioridad`. Es **ordinal: 1 = lo primero, gana el MENOR** — tal como la gente lo dice ("primero el semestre, segundo el examen, tercero el inglés"). Determina quién reserva horas antes en la validación de viabilidad y quién gana los desempates del PASO 5.5.
   **No hace falta prever reordenamientos futuros:** cuando un objetivo se complete, el siguiente sube solo. Si el usuario dice "cuando acabe el semestre quiero que la UNI pase a ser lo primero", eso ya pasa automáticamente — díselo en vez de inventar un mecanismo.

8. **Cursos compartidos y crecimiento del grafo**

   `cursos_requeridos` lista los cursos de los que el objetivo **trata**, no todo lo que toca. Lo que haga falta de otros cursos entra solo a través de los `prerrequisitos_externos` de sus temas — y solo los temas nombrados. Un objetivo de aprobar un semestre requiere sus cursos universitarios; de la base arrastra tres temas de geometría, no los trece.

   Si el objetivo nuevo trata de cursos que ya existen (el Paso 2 de análisis de reutilización te lo dice), **añade esas carpetas a su `cursos_requeridos` sin duplicar nada**: es muchos-a-muchos. No copies el curso, no lo muevas, no crees una versión paralela.

   Y cuando un curso nuevo necesite algo que no existe, aplica la **regla 12 del checklist de calidad** (crecimiento desde la necesidad hacia atrás):
   - Falta un tema en un curso que existe → agrégalo a ESE curso.
   - Falta el curso entero → créalo **solo con los temas necesarios**, no con los que "debería tener".
   - Hazlo **transitivamente**, parando al llegar a algo que el usuario ya tiene `completado`.

---

## Paso 2: Análisis de reutilización

Antes de generar cursos nuevos, analiza qué del plan actual sirve para el nuevo objetivo:

1. **Lee todos los `temario.json` existentes** y sus `progreso.json` asociados.
2. Identifica temas **ya completados** que sean prerrequisitos del nuevo objetivo.
3. Identifica temas **existentes pero no completados** que sean prerrequisitos → deben priorizarse.
4. Identifica **cursos completos que faltan** para el nuevo objetivo.
5. Identifica **temas nuevos que hay que agregar a cursos existentes** (ej: si el curso de cálculo 1 existe pero le faltan temas avanzados que el nuevo objetivo necesita).

Presenta al usuario un resumen tipo:

```
Análisis para tu nuevo objetivo "Aprobar cálculo 2":

Ya tienes cubierto:
- Álgebra completa (12/12 temas)
- Trigonometría completa (8/8 temas)
- Cálculo 1 completo (10/10 temas)

Cursos nuevos que necesitas:
- Cálculo 2 — Nuevo (por generar)

Temas nuevos a agregar en cursos existentes:
- (Ninguno en este caso)

Estimación: 1 curso nuevo, ~28 sesiones adicionales.
```

---

## Paso 3: Generar solo lo nuevo

Aplica el **mismo checklist de calidad del temario** definido en el `README.md` raíz para los cursos/temas nuevos. Reglas específicas de ampliación:

- **Para cursos nuevos completos**: crea la estructura copiando desde `_plantillas/` (igual que Paso 4 de `entrevista.md`).
- **Decide el `modo_estudio` de cada curso nuevo y confírmalo con el usuario**, siguiendo el Paso 5c de `entrevista.md`. La plantilla trae `tipo: "ruta"` por defecto: **el default no es una decisión.** Un módulo de habilidad (un idioma, por ejemplo) que se quede en `ruta` por omisión nunca se intercalará entre las clases y acabará estudiándose como una secuencia lineal, que es justo lo que no quieres de él. Si el curso nuevo es de `cadencia`, pregunta cada cuántos días debe tocarse.
- **NO regeneres** los `temario.json` de cursos existentes salvo que haya que agregar temas nuevos (en cuyo caso, agrega solo los nuevos temas al array, no reescribas los existentes).
- **Los temas ya completados** de cursos existentes NO aparecen como prerrequisitos internos del temario nuevo (ya están cumplidos), pero SÍ pueden aparecer como `prerrequisitos_externos` en el README del curso nuevo.
- **Bibliografía**: puede compartir libros con cursos anteriores o traer nuevos.
- **Sugerencia proactiva:** antes de generar carpetas o modificar `objetivo.json`, ofrece al usuario ejecutar `backup` (ver `_protocolos/utilidades.md`) para tener un snapshot previo por si algo sale mal.

---

## Paso 4: Actualizar `objetivo.json`

Actualiza el archivo con la nueva estructura:

Ejemplo con **dos objetivos en paralelo** (el caso que este protocolo debe soportar):

Ejemplo con **tres objetivos en paralelo**, uno de ellos latente y con cursos compartidos (el caso que este protocolo debe soportar):

```json
{
  "presupuesto_horas": [
    { "desde": "2026-07-16", "hasta": "2026-08-09", "horas_semanales": 35, "nota": "vacaciones" },
    { "desde": "2026-08-10", "hasta": null,         "horas_semanales": 15, "nota": "con clases encima" }
  ],
  "objetivos": [
    {
      "id": "obj-2",
      "estado": "activo",
      "condicion_activacion": null,
      "prioridad": 1,
      "objetivo_final": "Aprobar el 4to semestre de Ing. Mecatrónica en la UNFV",
      "fecha_limite": "2026-11-28",
      "hitos": [
        { "id": "parcial", "nombre": "Evaluación Parcial", "fecha": "2026-09-28",
          "cubre": "Primera mitad de cada curso (semanas 1-8)", "condicional": false },
        { "id": "final", "nombre": "Evaluación Final", "fecha": "2026-11-23",
          "cubre": "Segunda mitad (semanas 8-16). NO acumulativo.", "condicional": false },
        { "id": "sustitutorio", "nombre": "Sustitutorio", "fecha": null,
          "cubre": "TODO el semestre. Solo si se jala una nota.", "condicional": true }
      ],
      "estado_setup": "completo",
      "cursos_requeridos": ["04-trigonometria", "05-fisica", "09-mate-superior", "10-estatica"]
    },
    {
      "id": "obj-1",
      "estado": "activo",
      "condicion_activacion": null,
      "prioridad": 2,
      "objetivo_final": "Ingresar a la UNI vía examen de admisión 2027-1",
      "fecha_limite": "2027-02-15",
      "hitos": [
        { "id": "examen-uni", "nombre": "Examen de admisión", "fecha": "2027-02-15",
          "cubre": "Todo el temario de sus cursos requeridos", "condicional": false }
      ],
      "estado_setup": "completo",
      "cursos_requeridos": ["01-aritmetica", "04-trigonometria", "05-fisica", "07-aptitud-academica"]
    },
    {
      "id": "obj-3",
      "estado": "latente",
      "condicion_activacion": "Cuando obj-1 pase a estado 'completado' (tras el examen del 15.02.2027, sea cual sea el resultado).",
      "prioridad": 4,
      "objetivo_final": "Dominar de verdad las bases antes del 5to semestre",
      "fecha_limite": null,
      "hitos": [],
      "estado_setup": "completo",
      "cursos_requeridos": ["11-dibujo-ingenieria", "12-geometria-descriptiva"]
    }
  ]
}
```

Fíjate en que `04-trigonometria` y `05-fisica` están en **dos** objetivos a la vez. Eso es correcto y deliberado.

**Reglas:**
- El objetivo nuevo se **agrega al array** `objetivos` con un `id` incremental (`obj-2`, `obj-3`, …). El orden del array da igual: manda `prioridad`.
- Un objetivo pasa a `estado: "completado"` (con su `fecha_completado`) **solo cuando el usuario lo da por terminado** — nunca porque llegó uno nuevo. Se queda en el array: su historia es evidencia del conocimiento.
- **No hay `horas_semanales` por objetivo.** Compiten por `presupuesto_horas` mediante la cola con prioridad (ver "El presupuesto" en el `README.md` raíz). Si ves `horas_semanales` dentro de un objetivo, o un `horas_semanales_disponibles` escalar en la raíz, es deuda de un esquema viejo: bórralo.
- **Al añadir un objetivo, revisa si `presupuesto_horas` sigue siendo cierto.** Un objetivo nuevo suele venir con un cambio de vida (empieza un semestre, acaba un trabajo). Pregunta si la disponibilidad cambia y añade periodos si hace falta.
- **Pon el `liston` del objetivo nuevo** (`dominar` o `pasar`). Si comparte cursos con otro objetivo de listón distinto, avisa al usuario del coste del cobro por diferencia ANTES de cerrar la ampliación.
- `cursos_requeridos` es **muchos-a-muchos**. Un curso en varios objetivos NO es un error: es lo normal y lo que hace que estudiarlo una vez rinda dos veces. **No lo dupliques ni lo muevas para "resolver" el solapamiento.**
- **Nunca sumes las `sesiones_estimadas` de dos objetivos que comparten cursos** para estimar el total: cuenta doble. Usa el algoritmo de la cola, que cobra cada tema una vez.

---

## Paso 5: Cerrar la ampliación

1. Marca `estado_setup: "completo"` en el objetivo nuevo dentro de `objetivos`.
2. **Re-valida la viabilidad de TODOS los objetivos activos**, no solo del nuevo, con el algoritmo de la cola con prioridad (regla 10 del checklist). Meter un objetivo nuevo por delante de otro le quita horas al de atrás: un plan que antes cabía puede haber dejado de caber. **Si eso pasa, dilo AHORA**, antes de dar por cerrada la ampliación: *"Con el semestre por delante, la UNI ya no entra para febrero: faltan ~40 h. ¿Recortamos opcionales, movemos algo, o aflojamos la cadencia de inglés?"* Este aviso es el punto entero de este paso.
3. Actualiza el `README.md` raíz si es necesario (diagrama de dependencias, etc.).
4. Muestra al usuario el plan completo: qué objetivos quedan activos y en qué orden, qué hitos tiene cada uno y cuál es el más cercano, y **qué cursos se comparten** (esos son los que rinden doble). Sugiere por dónde arrancar. Recuérdale que el PASO 5.5 repartirá las clases según prioridad, hitos y cadencias.

A partir de aquí, las sesiones siguen el protocolo normal del `README.md` raíz.

---

## Notas importantes

- **Si por alguna razón el directorio `_ejemplo-algebra/` sigue existiendo** (no debería, si el setup inicial se completó correctamente), elimínalo antes de continuar. En una ampliación ya no estamos en setup inicial.
- **Detecta duplicación**: si el usuario propone algo que ya está en un curso existente, avísale — no generes un curso duplicado.
- **Confirma con el usuario antes de crear/modificar archivos**. Un plan es un compromiso — que él lo apruebe primero.
