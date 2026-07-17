# Protocolo de primer arranque (setup inicial)

> Este archivo lo lee el agente de IA la primera vez que el usuario abre el proyecto — específicamente cuando detecta que **no existe** `objetivo.json` en la raíz. Guía la conversación para generar todo el plan de estudio personalizado.

---

## Cuándo aplicar este protocolo

Al inicio de cualquier interacción, revisa si existe `objetivo.json` en la raíz:

- **Si NO existe** → estás en modo SETUP. Sigue este archivo paso a paso antes de cualquier otra cosa.
- **Si existe y TODOS los objetivos con `estado: "activo"` tienen `estado_setup: "completo"`** → ignora este archivo, sigue el protocolo normal del `README.md` raíz.
- **Si existe y ALGUNO tiene `estado_setup: "incompleto"`** → retoma el setup de ESE objetivo desde donde quedó (revisa qué pasos ya se cumplieron). No toques los objetivos que ya están completos.

---

## Paso 0: Limpiar el ejemplo del template

**ANTES de cualquier otra acción**, verifica si existe el directorio `_ejemplo-algebra/` en la raíz.

Ese directorio es el **curso de referencia del template**, NO un curso del usuario. Debe eliminarse en el primer setup para que no aparezca como un curso "real" en el plan del usuario.

- **Si existe `_ejemplo-algebra/`** → confirma con el usuario ("Voy a eliminar el curso de referencia `_ejemplo-algebra/` que viene con el template — es solo un ejemplo. ¿Confirmas?") y elimínalo por completo.
- **Si NO existe** → ya fue eliminado en un setup anterior, continúa al Paso 1.

---

## Paso 1: Entrevista al usuario

Preséntate como el tutor y explícale que necesitas conocer su objetivo antes de armar el plan. Haz las siguientes preguntas **una por una**, no todas juntas. Espera respuesta antes de la siguiente.

1. **Objetivo final**
   "¿Qué quieres aprender o lograr con este plan de estudio? Sé lo más concreto posible."
   *(Ej: "Aprobar cálculo I en la universidad", "Entender fundamentos de ciberseguridad para trabajar como pentester", "Aprender música desde cero para tocar guitarra clásica")*

2. **Fecha límite**
   "¿Tienes una fecha límite? ¿Para cuándo necesitas dominar esto?"
   *(Si no hay fecha, marcar `fecha_limite: null` y no aplicar validaciones de tiempo)*

3. **Horas semanales disponibles**
   "¿Cuántas horas por semana puedes dedicarle de forma realista?"
   *(No aceptar respuestas irrealistas — si dice 40h/semana y trabaja full time, cuestiona con amabilidad)*

4. **Nivel actual**
   "¿Qué sabes ya sobre este tema o áreas relacionadas? Sé honesto — es mejor sobreestimar lo que no sabes que lo contrario."
   *(Esto define desde qué punto arranca el árbol de cursos)*

5. **Motivación**
   "¿Por qué quieres aprender esto? Contexto ayuda a personalizar el enfoque."
   *(Ej: universidad, cambio de carrera, curiosidad, examen específico)*

Al terminar, muestra al usuario un resumen y pide confirmación antes de crear `objetivo.json`.

---

## Paso 2: Generar `objetivo.json` y `configuracion.json`

Para `objetivo.json`:
1. Copia el archivo `_plantillas/plantilla-objetivo.json` a la raíz como `objetivo.json`.
2. Reemplaza los valores placeholder con los reales de la entrevista, en el primer (y único) objetivo del array `objetivos`.
3. Marca su `estado_setup: "incompleto"` — solo pasa a `"completo"` cuando termine TODO el setup (Paso 6).
4. `estado: "activo"` y `condicion_activacion: null`. (Un objetivo `latente` solo aparece en ampliaciones, no en el setup inicial.)
5. Pon su `fecha_inicio` con la fecha de hoy y su `cursos_requeridos` como array vacío (se poblará en Paso 4).
6. Llena `presupuesto_horas` (raíz). **Es una línea de tiempo de periodos, no un número.** Pregunta al usuario si su disponibilidad va a cambiar en el horizonte del plan (vacaciones, un semestre que arranca, un trabajo que empieza o acaba) y crea un periodo por cada tramo distinto. Si es estable, un solo periodo con `"hasta": null` basta — pero **pregúntaselo, no lo asumas**: un escalar plano promedia mundos distintos y miente en ambas direcciones. **No hay presupuesto por objetivo**: los objetivos compiten por este (ver "El presupuesto" en el `README.md` raíz).
6b. **No hay un `liston` de objetivo.** Lo que antes era `dominar`/`pasar` ahora se declara con MÁS grano: cada entrada de `cursos_requeridos` lleva su `nivel_requerido` (`base` / `intermedio` / `avanzado`) y `overrides` por tema. Aún no pobles esto (los cursos se crean en el Paso 4), pero decide ya, con el usuario, el **listón mental** del objetivo — "¿esto es para dominarlo de verdad o para aprobar y seguir?" — porque de ahí saldrá el `nivel_requerido` por defecto que pondrás a cada curso: "pasar" ≈ `base`/`intermedio`, "dominar" ≈ `avanzado`. Ver "El conocimiento se mide en DOS dimensiones" en el `README.md` raíz.
7. `prioridad: 1`. Es ordinal (1 = lo primero) y solo importa cuando haya más de un objetivo activo.
8. Llena `hitos` con las fechas de corte reales del objetivo. Si solo hay una (un examen, una entrega), **un solo hito basta** — no inventes cortes que no existen. Si el objetivo no tiene ninguna fecha, deja el array vacío y `fecha_limite: null`.

> **Nota:** `objetivos` es un **array** aunque en el setup inicial solo tenga un elemento. El usuario puede añadir objetivos paralelos después, con ciclos de vida independientes. No lo colapses a un objeto único ni asumas que siempre habrá uno. Ver "Objetivos paralelos" en el `README.md` raíz.

Para `configuracion.json` (si aún no existe en la raíz):
1. Copia `_plantillas/plantilla-configuracion.json` a la raíz como `configuracion.json`.
2. Los valores por defecto están bien para la mayoría de usuarios (ver `_protocolos/configuracion.md` para ajustarlos vía comandos).

**Regla general:** siempre que necesites crear un archivo estructurado del proyecto, primero busca su plantilla en `_plantillas/` y cópiala en vez de escribir la estructura a mano. Así, cambios futuros al esquema se propagan desde un único lugar.

---

## Paso 3: Proponer la lista de cursos necesarios

En base al objetivo y nivel actual, arma una lista de cursos que el usuario necesita, ordenados por dependencias.

**Reglas:**

1. **Ingeniería inversa desde el objetivo**: parte del conocimiento final requerido y pregunta recursivamente "¿qué se necesita saber antes?". No listar cursos al azar.
2. **Considera el nivel actual**: si dice que ya domina X, no incluyas X como curso.
3. **Cursos básicos primero, integradores después**: los cursos que combinan varios (ej: física combina cálculo + álgebra + geometría) van al final.
4. **Nombra los cursos claramente**: nada de nombres genéricos como "Curso 1". Usa nombres reales del área.
5. **Máximo 8 cursos** en el plan inicial. Si necesitas más, algo está mal descompuesto.

Presenta la lista al usuario con este formato:

```
Plan propuesto para tu objetivo:

1. [Nombre] — Básico — [breve razón de por qué está]
2. [Nombre] — Básico — [...]
3. [Nombre] — Integrador — Requiere: 1, 2
...

Estimación total: N cursos, ~M sesiones.
```

Espera que el usuario apruebe, quite o agregue cursos antes de continuar.

---

## Paso 4: Crear las carpetas de cursos

Para cada curso aprobado, crea la estructura copiando desde `_plantillas/`:

```
NN-nombre-curso/
├── README.md           ← copia de _plantillas/plantilla-README-curso.md, adaptar {{CURSO}} y prerrequisitos
├── progreso.json        ← copia de _plantillas/plantilla-progreso.json (poblará en Paso 5 tras generar temario.json)
├── glosario.json        ← array vacío []
├── dificultades.json    ← array vacío []
└── temario.json         ← SE GENERA EN PASO 5 desde _plantillas/plantilla-temario.json
```

Numera secuencialmente (`01-`, `02-`, etc.) respetando el orden de dependencias.

En el `README.md` de cada curso puedes declarar los **prerrequisitos externos** en prosa, como resumen para el usuario. Pero la verdad operativa va **a nivel de tema**, en `prerrequisitos_externos` de cada tema del `temario.json` (formato `"curso/tema_id"`) — ver la regla 4 del checklist de calidad. Un prerrequisito declarado solo a nivel de curso arrastra el curso entero a la planificación e infla el plan.

Registra los nombres de las carpetas creadas en el `cursos_requeridos` **del objetivo que los necesita**, dentro de `objetivos` en `objetivo.json`. **Cada entrada lleva su `nivel_requerido`** (`base` / `intermedio` / `avanzado`, según el listón que decidiste en el Paso 6b) y, si un tema concreto es un cuello de botella del que cuelga otro curso, un `override` que lo suba por encima del default. Esa lista es la única fuente de verdad de qué objetivo requiere qué curso Y a qué nivel — no dupliques el dato dentro de los `temario.json`. Recuerda que es **muchos-a-muchos**: si más adelante otro objetivo necesita el mismo curso a otro nivel, se añade a su lista con SU `nivel_requerido`, sin copiar la carpeta.

---

## Paso 5: Proponer bibliografía y generar `temario.json`

Este es el paso más crítico. Hazlo **curso por curso**, no todos a la vez.

### 5a. Proponer bibliografía

Para el curso actual, propón 2-3 libros/recursos estándar del área. Ejemplos por área:

- Matemáticas: Baldor (álgebra), Stewart (cálculo), Purcell, Larson
- Física: Serway, Halliday-Resnick, Sears-Zemansky
- Programación: Structure and Interpretation, Clean Code, libros oficiales de cada lenguaje
- Ciberseguridad: The Web Application Hacker's Handbook, PTES, OWASP guides

Pregunta al usuario:
- "¿Tienes acceso o preferencia por alguno de estos?"
- "¿O prefieres aportar tú un libro/curso específico que ya estés usando?"

Guarda la bibliografía elegida — irá dentro de `temario.json`.

### 5b. Generar `temario.json` aplicando el checklist de calidad

Sigue el **checklist obligatorio** definido en la sección "Reglas de calidad del temario" del `README.md` raíz. **Léelo entero antes de escribir el primer tema; no lo cites de memoria.** No inventes temas al azar ni copies índices literales de libros. Aplica ingeniería inversa desde el objetivo del curso.

Presta atención especial a tres reglas que son fáciles de saltarse y caras de arreglar después:
- **Regla 4** — prerrequisitos SIEMPRE a nivel de tema (`prerrequisitos_internos` y `prerrequisitos_externos`), nunca a nivel de curso.
- **Regla 11** — `modo_estudio` es obligatorio. Sin él el curso queda huérfano y nunca se estudia.
- **Regla 12** — el grafo crece desde la necesidad hacia atrás. **Solo los temas que hacen falta**, no los que "debería tener" un curso de esa materia. Un curso creado para dar soporte a otro puede tener tres temas y estar perfecto. El criterio de parada es llegar a algo que el usuario ya domina, no que el curso "se vea completo".

Muestra el temario al usuario y pide ajustes antes de guardarlo.

### 5c. Decidir el `modo_estudio` del curso

**No te saltes esto y no lo dejes en el default.** La plantilla trae `tipo: "ruta"`, y si nadie decide, el curso se queda en ruta por omisión — no porque sea lo correcto. Un módulo que debía estudiarse en dosis frecuentes acaba tratado como una secuencia lineal, y nunca se intercala. Ver "Cómo se estudia cada curso" en el `README.md` raíz para la estructura completa del campo.

Clasifica el curso y **propónselo al usuario para que confirme**:

- **`ruta`** — el curso es una secuencia acumulativa: cada tema se apoya en el anterior y el objetivo es recorrerlo entero. Casi todo lo académico (matemáticas, física, química, un lenguaje de programación).
- **`cadencia`** — el curso es una **habilidad que se entrena**, no un cuerpo de contenido que se cubre: poca teoría, mucha práctica repetida, y rinde mucho más en dosis pequeñas y frecuentes que en bloques largos. Idiomas, razonamiento verbal y matemático, mecanografía, un instrumento. Pregunta al usuario cada cuántos días quiere tocarlo y ponlo en `cadencia_dias`.
- **`hito`** — el curso solo tiene sentido cuando otra cosa ya está lista: simulacros, exámenes de práctica, proyecto final, integración. Requiere `condicion` y `condicion_urgente`, y las dos deben ser **aritmética verificable** (ver la regla 11 del checklist de calidad).

Pregunta directamente cuando dudes: *"¿Esto lo quieres llevar como un curso que se recorre de principio a fin, o como algo que tocas un rato cada pocos días?"* La respuesta del usuario manda sobre tu clasificación.

Asigna también `prioridad` (entero): desempata cuando dos cursos compiten por la misma sesión. Para cursos `ruta`, refleja el orden de dependencias (los que son base de otros, más alto).

### 5d. Validación temporal

Después de generar el temario, calcula la viabilidad **contando las dos partidas** — aprender y mantener — porque el mantenimiento cuesta horas reales y un plan que solo cuenta el aprendizaje miente diciendo "cabe" cuando no cabe:

```
horas_aprender   = Σ (saltos de nivel necesarios por tema, con sus sesiones_por_nivel,
                      hasta el nivel_requerido del objetivo) × HORAS_POR_SESION × 1.2 (buffer)
horas_mantener   = (nº de repasos que la escalera dispara entre el cierre estimado de cada
                    tema y la fecha del hito) × minutos_por_repaso / 60
tiempo_estimado  = horas_aprender + horas_mantener
tiempo_disponible = integral de presupuesto_horas sobre [hoy, fecha_del_hito]
                    (NO es una multiplicación: los periodos tienen ritmos distintos)
```

`HORAS_POR_SESION` sale de `historial.json` (promedio de `duracion_min`/60) y `minutos_por_repaso` también (ver "El mantenimiento CUESTA HORAS" y la regla 10 del checklist de calidad en el `README.md` raíz). **No cuentes el 70/30 del intercalado aparte de la escalera: son el mismo fenómeno.**

**En el setup inicial el historial está vacío por definición**, así que aquí usarás los provisionales de 1.5 h/sesión y 12 min/repaso. **Dilo explícitamente al usuario:** *"esta estimación asume sesiones de 1.5 h y repasos de 12 min; en unas semanas el historial dirá tu ritmo real y la reviso"*. Presentar una viabilidad calculada sobre constantes inventadas como si fueran un hecho es la forma más fácil de que un plan nazca mintiendo.

Si `tiempo_estimado > tiempo_disponible`:
- Reporta el desfase al usuario, separando cuánto es aprender y cuánto mantener.
- Propón opciones: (a) marcar temas como `opcional: true`, (b) bajar el `nivel_requerido` de algún curso (aplaza trabajo, no lo elimina), (c) aumentar horas/semana, (d) extender el hito si es negociable.
- **Predicción de olvido:** avisa además de los temas que, al ritmo de repaso previsto, llegarán fríos al hito (su `nivel_repaso` no aguanta la distancia hasta la fecha). Es obligatorio, no opcional.

Repite el paso 5 para cada curso de la lista.

---

## Paso 6: Cerrar el setup

Cuando todos los cursos tengan su `temario.json` y el usuario haya aprobado el plan completo:

1. Actualiza `objetivo.json`: `estado_setup: "completo"` en el objetivo dentro de `objetivos`.
2. Muéstrale al usuario el resumen final: cuántos cursos, sesiones totales, fecha estimada de finalización.
3. Sugiere por dónde arrancar (el primer curso sin prerrequisitos).

**A partir de este punto**, cualquier futura sesión sigue el protocolo normal del `README.md` raíz.

Si en el futuro el usuario quiere agregar un objetivo nuevo —ya sea porque completó este o porque quiere perseguir otro **en paralelo**— consulta `_protocolos/expansion.md` en lugar de este archivo.

---

## Notas importantes

- **No ejecutes el setup a la fuerza** si el usuario dice que solo quiere explorar. Pregunta primero.
- **Guarda progreso**: si el usuario interrumpe el setup a mitad, guarda `objetivo.json` con lo que haya y `estado_setup: "incompleto"`. Retoma después.
- **No inventes cursos que no sean necesarios**: el instinto de "por si acaso" hincha el plan y desmotiva al usuario. Recorta al mínimo viable para lograr el objetivo.
- **Confirma antes de eliminar o crear directorios**. El Paso 0 (eliminar `_ejemplo-algebra/`) requiere confirmación explícita del usuario.
