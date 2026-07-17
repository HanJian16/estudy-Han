# PENDIENTE — Migración al modelo de dos dimensiones (abierta desde 2026-07-16)

> **Este archivo es temporal.** Existe porque el 2026-07-16 se rediseñó el núcleo del proyecto y **los datos se migraron pero la documentación quedó a medias**. Bórralo cuando todo lo de aquí esté cerrado.
>
> **Si eres un agente que abre una sesión nueva: lee esto ANTES de tocar nada.** El `README.md` se contradice a sí mismo ahora mismo y necesitas saber qué parte manda.

---

## ⚠ El estado actual en una frase

**Los datos usan el modelo nuevo. Media documentación describe el viejo. Y el campo central del modelo nuevo (`nivel_alcanzado`) no lo escribe nadie**, porque el "Protocolo al CERRAR una clase" todavía no sabe que existe. Hasta que eso se arregle, **el rediseño es decorativo**.

**Qué manda:** el modelo nuevo, documentado en el README en las secciones "El conocimiento se mide en DOS dimensiones", "La durabilidad" y "El mantenimiento CUESTA HORAS". Todo lo que contradiga eso es deuda pendiente de borrar.

---

## El modelo nuevo, en corto

Un tema tiene **dos coordenadas ortogonales**, y ninguna es "completado":

- **Profundidad** → `nivel_alcanzado` en `progreso.json`: `null` / `base` / `intermedio` / `avanzado`. Es un **estado verificable**: solo sube si se cumple el `criterio_dominio` de ese nivel. Nunca sube por haber gastado sesiones.
- **Durabilidad** → `nivel_repaso` (0-5): cuánto va a aguantar (1≈1 día, 2≈3, 3≈7, 4≈14, 5≈30).

Cada objetivo declara `cursos_requeridos` con `nivel_requerido` por curso y `overrides` por tema. **"Completado" es derivado y relativo**: un tema está completo *para un objetivo* si `nivel_alcanzado >= nivel_requerido` por ese objetivo. Nunca en absoluto.

---

## Trabajo pendiente

### 1. Contradicciones activas en `README.md` (lo más urgente)

- [ ] **Borrar la sección "El listón"** (~línea 445). Está superada por "El conocimiento se mide en DOS dimensiones". Describe `liston`, `sesiones_estimadas_pasar` y un cobro por diferencia **en horas** que ya no existe (ahora es por nivel). **Dos modelos vivos en la fuente de verdad.**
- [ ] **Cerradura** (~línea 491): para en `estado == "completado"`. Debe parar en `nivel_alcanzado >= nivel_requerido por el objetivo que pregunta`.
- [ ] **Escalera de repaso** (~línea 583): dice "cuando un tema pasa a `completado` se le arma el repaso". Ese estado ya no significa eso → reescribir en términos de nivel alcanzado.
- [ ] **Re-planificación** (~726-749): razona en `sesiones_estimadas` y propone "bajar el `liston`". Traducir a niveles.
- [ ] **PASO 5.5** (~línea 180): exige "prerrequisitos internos completados", sin semántica de nivel.
- [ ] **Doc de campos de `progreso.json`** (~línea 975): desactualizada.

### 2. `Protocolo al CERRAR una clase` — SIN ESTO NADA FUNCIONA

- [ ] Debe **evaluar el `criterio_dominio` del nivel objetivo y actualizar `nivel_alcanzado`** — subiéndolo solo si se cumple. Hoy solo toca `estado` y `porcentaje`.
- [ ] Debe **escribir el `criterio_dominio` específico del tema para ese nivel** si aún no existe (ver punto 5).
- [ ] Armar la escalera en función del nivel alcanzado, no de `estado: completado`.

### 3. Matar `porcentaje`

- [ ] Eliminar el campo de `progreso.json` (13 cursos), la plantilla y toda referencia. **Duplica mal a `nivel_alcanzado`**: dos campos midiendo lo mismo con distinta precisión = deriva garantizada. Decisión tomada 2026-07-16.

### 4. Protocolos con referencias al modelo viejo

- [ ] `entrevista.md`: paso 6b manda poner un `liston` que ya no existe; la validación temporal usa `sesiones_estimadas`.
- [ ] `expansion.md`: paso 7 pide `liston`; regla sobre "no sumes `sesiones_estimadas`".
- [ ] `utilidades.md`: el comando `estado` lee `liston` y `sesiones_estimadas`. Debe mostrar **niveles alcanzados vs requeridos por objetivo**, y la predicción de olvido.
- [ ] `handoff.md`: reporta `porcentaje`; trata los intercalados como "temas `completado`". **Además: el handoff de VUELTA debe traer el nivel alcanzado y su verificación**, no solo un porcentaje. Una sesión externa tiene que poder decir "llegó a base pero no a intermedio".

### 5. Rúbricas de nivel por curso (13, no 450 criterios)

- [ ] Escribir en cada `temario.json` una **rúbrica de curso** que defina qué significa cada nivel *en ese curso*.

**REGLA DE DISEÑO (decidida por el usuario, 2026-07-16):** los niveles se expresan **como capacidades, no como el puntaje en el examen de un tercero**. Un borrador anterior usaba "8/10 tipo UNI" como vara de `avanzado`; se descartó porque **ata la medición a una entidad externa** y solo funciona donde esa entidad está bien calibrada — UNI sirve para matemática, pero no para lenguaje ni razonamiento verbal, y menos aún para inglés o para lo que venga. Escribir así:

- `base` — resuelve lo directo del tema y reconoce cuándo aplica.
- `intermedio` — resuelve ejercicios variados sin ayuda y lo combina con otros temas.
- `avanzado` — reconoce el tipo de problema al leerlo, resuelve bajo presión de tiempo, aguanta los casos raros.

Adáptalo a cada curso, pero **nunca lo ancles a la nota de nadie**.

- [ ] El **criterio específico por tema y nivel NO se fabrica por adelantado**: se escribe cuando el tema se estudia, que es justo cuando se necesita para decidir si el nivel sube (y ni un minuto antes). Hoy casi todos los temas tienen `criterio_dominio` como **un solo string**: es el criterio del nivel para el que se escribió el curso (`avanzado` en los base, `intermedio` en los universitarios). Trátalo así hasta que se complete.

### 6. Hacer funcional lo que hoy es solo prosa

> **Principio del usuario (2026-07-16):** *"si hemos hecho mejoras, no pueden quedarse como texto: tienen que ser funcionales"*. Todo lo de esta sección está **documentado pero no operativo** — nadie lo ejecuta.

- [ ] **Escalera adaptativa al hito.** El README explica que la durabilidad necesaria depende de la distancia al hito (un parcial a 10 semanas no necesita llegar a nivel 5; un examen a 7 meses sí). Pero el algoritmo sigue usando la escalera fija 1-3-7-14-30 para todo.
- [ ] **Predicción de olvido.** Documentada como aviso obligatorio en "La durabilidad", pero **no está en ningún protocolo**, así que nadie la va a dar. Debe entrar en `utilidades.md` (comando `estado`) y en el PASO 5.5.
- [ ] **Coste de mantenimiento en las validaciones.** El README ya manda sumar `horas_aprender + horas_mantener`, pero `entrevista.md` y `expansion.md` siguen calculando solo el aprendizaje.
- [ ] **`minutos_por_repaso`** debe medirse desde `historial.json` igual que `horas_por_sesion`. Hoy es un 12 inventado.

### 7. Auditoría de flujos (pedida por el usuario)

- [ ] **Recorrer los flujos completos de punta a punta** —arranque, clase normal, cierre, handoff de ida y vuelta, `estado`, expansión— y verificar que cada regla nueva **se ejecuta de verdad** y que no quedó nada roto o huérfano. Especial atención a:
  - **Sesión interrumpida a medias** (aquí o vía handoff): al retomarla, el nivel y el progreso deben quedar coherentes. Si una clase se corta antes de verificar el criterio, `nivel_alcanzado` NO debe subir.
  - Que ningún campo nuevo quede sin escritor (el caso de `nivel_alcanzado` hoy).
  - Que ninguna regla nueva quede sin nadie que la invoque (el caso de la predicción de olvido).

---

## Y aparte: portar los cambios al TEMPLATE de origen

Este proyecto nació de un **template genérico** (el que trae `_protocolos/`, `_plantillas/`, el algoritmo de arranque y el `_ejemplo-algebra/`). Todo el rediseño del 2026-07-16 es **mejora de arquitectura, no contenido personal**, así que debe volver al template para que cualquier usuario nuevo lo tenga.

**Qué portar** (configuración, protocolos, plantillas y reglas):

- El PASO 5.5 completo (selector de clase) y el canario.
- `modo_estudio` (ruta / cadencia / hito) y las condiciones como aritmética verificable.
- La escalera de repaso y el intercalado 70/30 con su coste.
- `historial.json` y la medición de `horas_por_sesion` / `minutos_por_repaso`.
- El array `objetivos` con `estado` (activo/latente/completado) y `condicion_activacion`.
- `cursos_requeridos` muchos-a-muchos con niveles y overrides.
- `presupuesto_horas` como línea de tiempo de periodos.
- Los `hitos` por objetivo, con condicionales.
- Prerrequisitos externos a nivel de tema y la cerradura transitiva.
- El modelo de dos dimensiones entero.
- `una_clase_por_sesion` (antes `un_tema_por_sesion`) y la contención de alcance.
- Las reglas 4, 11 y 12 del checklist de calidad.

**Qué NO portar:** nada de los 13 cursos, ni `objetivo.json` con las metas reales, ni el historial, ni las clases. El template no tiene cursos: tiene `_ejemplo-algebra/` como muestra.

**Cómo:** generar un prompt autocontenido para una sesión en el repo del template, que explique cada cambio **con su porqué** (los "por qué" están en las notas de cambio del README y de las plantillas — son la parte valiosa). Revisar el historial de git de este repo para no olvidar nada.

---

## Contexto que no está en ningún archivo y conviene no perder

- **La conversación de las horas quedó pendiente a propósito**, para el final, porque depende de que todo lo demás esté cerrado. Estado: con el modelo honesto, **nada cabe** — parcial −102 h, final −205 h, UNI −281 h (~590 h de déficit). Las opciones sobre la mesa: retirar cursos del semestre, no reestudiar la base, aceptar jalar algo, o mover metas. **Aún sin decidir.**
- Todos esos números descansan sobre supuestos sin medir: 1.5 h/sesión, 12 min/repaso, el reparto mecánico de `sesiones_por_nivel`, y 35/15 h semanales que son promesas. **El checkpoint del 26 de julio** (10 días de historial) es la primera medición real.
- El usuario decidió **bajar el listón de la base ya**, sin esperar al 26, porque el historial mide igual con cualquier listón y quemar la ventana de vacaciones al ritmo caro era el riesgo mayor.
- Los 5 temarios del semestre son **inferidos** y todos llevan la obligación de revisarse en la semana 1 de clases (10-14 agosto 2026) contra el sílabo real.
