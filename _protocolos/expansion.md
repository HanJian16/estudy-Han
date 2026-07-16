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

3. **Fecha límite y horas semanales**
   "¿Hay deadline? ¿Cuántas horas por semana puedes dedicarle ahora?"

4. **Estado del objetivo anterior**
   "¿Damos por completado el objetivo anterior o quieres mantener ambos activos en paralelo?"
   - Si dice completado → mueve el `objetivo_actual` a `objetivos_completados` en `objetivo.json` con `fecha_completado`.
   - Si dice en paralelo → mantén el anterior como `objetivo_actual` y agrega el nuevo también (aunque esto es raro y generalmente no recomendado; sugiere completar uno antes de arrancar otro).

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

```json
{
  "objetivo_actual": {
    "id": "obj-2",
    "objetivo_final": "Aprobar cálculo 2",
    "fecha_limite": "2027-06-01",
    "horas_semanales": 6,
    "nivel_actual_al_inicio": "Cálculo 1, álgebra, trigonometría dominados",
    "motivacion": "Continuidad de la carrera",
    "fecha_inicio": "2026-12-15",
    "estado_setup": "completo",
    "cursos_generados": ["04-calculo-2"]
  },
  "objetivos_completados": [
    {
      "id": "obj-1",
      "objetivo_final": "Aprobar cálculo 1",
      "fecha_limite": "2026-12-01",
      "fecha_inicio": "2026-06-30",
      "fecha_completado": "2026-12-05",
      "cursos_generados": ["01-aritmetica", "02-algebra", "03-trigonometria", "04-calculo-1"]
    }
  ]
}
```

**Reglas:**
- El objetivo anterior pasa al array `objetivos_completados` con su `fecha_completado`.
- El nuevo objetivo se convierte en `objetivo_actual` con un `id` incremental (`obj-2`, `obj-3`, ...).
- `cursos_generados` lista SOLO los cursos generados o expandidos por este objetivo (los históricos quedan asociados a su objetivo original).

---

## Paso 5: Cerrar la ampliación

1. Marca `estado_setup: "completo"` en el nuevo `objetivo_actual`.
2. Actualiza el `README.md` raíz si es necesario (sección de "curso actual", diagrama de dependencias, etc.).
3. Muestra al usuario el nuevo plan y sugiere por dónde arrancar (típicamente el primer tema del curso nuevo, o el primer tema pendiente que sea prerrequisito).

A partir de aquí, las sesiones siguen el protocolo normal del `README.md` raíz.

---

## Notas importantes

- **Si por alguna razón el directorio `_ejemplo-algebra/` sigue existiendo** (no debería, si el setup inicial se completó correctamente), elimínalo antes de continuar. En una ampliación ya no estamos en setup inicial.
- **Detecta duplicación**: si el usuario propone algo que ya está en un curso existente, avísale — no generes un curso duplicado.
- **Confirma con el usuario antes de crear/modificar archivos**. Un plan es un compromiso — que él lo apruebe primero.
