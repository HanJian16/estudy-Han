# Protocolo de primer arranque (setup inicial)

> Este archivo lo lee el agente de IA la primera vez que el usuario abre el proyecto — específicamente cuando detecta que **no existe** `objetivo.json` en la raíz. Guía la conversación para generar todo el plan de estudio personalizado.

---

## Cuándo aplicar este protocolo

Al inicio de cualquier interacción, revisa si existe `objetivo.json` en la raíz:

- **Si NO existe** → estás en modo SETUP. Sigue este archivo paso a paso antes de cualquier otra cosa.
- **Si existe y `objetivo_actual.estado_setup: "completo"`** → ignora este archivo, sigue el protocolo normal del `README.md` raíz.
- **Si existe pero `objetivo_actual.estado_setup: "incompleto"`** → retoma el setup desde donde quedó (revisa qué pasos ya se cumplieron).

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
2. Reemplaza los valores placeholder con los reales de la entrevista.
3. Marca `objetivo_actual.estado_setup: "incompleto"` — solo pasa a `"completo"` cuando termine TODO el setup (Paso 6).
4. Pon `objetivo_actual.fecha_inicio` con la fecha de hoy.
5. Deja `objetivos_completados` como array vacío y `objetivo_actual.cursos_generados` como array vacío (se poblará en Paso 4).

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

En el `README.md` de cada curso, declara los **prerrequisitos externos** (qué otros cursos de la lista deben completarse antes).

Registra los nombres de las carpetas creadas en `objetivo_actual.cursos_generados` en `objetivo.json`.

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

Sigue el **checklist obligatorio** definido en la sección "Reglas de calidad del temario" del `README.md` raíz. No inventes temas al azar ni copies índices literales de libros. Aplica ingeniería inversa desde el objetivo del curso.

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

Después de generar el temario, calcula:

```
tiempo_estimado = suma(sesiones_estimadas) × 1.5 horas × 1.2 (buffer)
tiempo_disponible = semanas_hasta_deadline × horas_semanales
```

Si `tiempo_estimado > tiempo_disponible`:
- Reporta el desfase al usuario.
- Propón opciones: (a) marcar temas como `opcional: true`, (b) aumentar horas/semana, (c) extender deadline.

Repite el paso 5 para cada curso de la lista.

---

## Paso 6: Cerrar el setup

Cuando todos los cursos tengan su `temario.json` y el usuario haya aprobado el plan completo:

1. Actualiza `objetivo.json`: `objetivo_actual.estado_setup: "completo"`.
2. Muéstrale al usuario el resumen final: cuántos cursos, sesiones totales, fecha estimada de finalización.
3. Sugiere por dónde arrancar (el primer curso sin prerrequisitos).

**A partir de este punto**, cualquier futura sesión sigue el protocolo normal del `README.md` raíz.

Si en el futuro el usuario completa este objetivo y quiere agregar uno nuevo, consulta `_protocolos/expansion.md` en lugar de este archivo.

---

## Notas importantes

- **No ejecutes el setup a la fuerza** si el usuario dice que solo quiere explorar. Pregunta primero.
- **Guarda progreso**: si el usuario interrumpe el setup a mitad, guarda `objetivo.json` con lo que haya y `estado_setup: "incompleto"`. Retoma después.
- **No inventes cursos que no sean necesarios**: el instinto de "por si acaso" hincha el plan y desmotiva al usuario. Recorta al mínimo viable para lograr el objetivo.
- **Confirma antes de eliminar o crear directorios**. El Paso 0 (eliminar `02-algebra/`) requiere confirmación explícita del usuario.
