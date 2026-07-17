# PENDIENTE — Migración al modelo de dos dimensiones

> **Estado (actualizado esta sesión):** la migración interna está **CERRADA**. El `README.md` ya NO se contradice a sí mismo: el modelo de dos dimensiones es la única verdad viva en toda la documentación y los datos. Este archivo se conserva solo por dos cosas que quedan fuera de este repo o pendientes de decisión (abajo). Bórralo cuando ambas se cierren.

---

## Lo que se cerró (rama `migracion-modelo-dos-dimensiones`)

Los 7 bloques de deuda de la migración están hechos y commiteados:

1. ✅ **`porcentaje` eliminado** de los 13 `progreso.json` y la plantilla (duplicaba a `nivel_alcanzado`).
2. ✅ **El "Protocolo al CERRAR una clase" escribe `nivel_alcanzado`** verificando el `criterio_dominio` del nivel — el escritor que faltaba. Sesión interrumpida no sube el nivel.
3. ✅ **Contradicciones del README resueltas**: borrada "El listón"; cerradura y semilla paran en `nivel_alcanzado >= nivel_requerido`; escalera atada a la profundidad; re-planificación y PASO 5.5 en lenguaje de niveles; doc de campos al día; `estado` redefinido como flujo puro (`no_iniciado`/`en_progreso`/`completado`, este último = mantenimiento, no conocimiento).
4. ✅ **Protocolos migrados**: `handoff.md` (VUELTA trae nivel + verificación, no porcentaje), `utilidades.md` (comando `estado` en niveles + predicción de olvido + mide `minutos_por_repaso`), `entrevista.md` y `expansion.md` (sin `liston`, con coste de mantenimiento y `cursos_requeridos` en forma objeto).
5. ✅ **Rúbricas de nivel por curso** en los 13 temarios y la plantilla (capacidades, nunca la nota de un tercero). El `criterio_dominio` por tema/nivel se deriva de la rúbrica cuando el tema se estudia.
6. ✅ **Prosa hecha funcional**: escalera adaptativa al hito, predicción de olvido (comando `estado` + PASO 5.5), coste de mantenimiento en las validaciones, `minutos_por_repaso` medido.
7. ✅ **Auditoría de flujos** punta a punta: ningún campo sin escritor, ninguna regla sin invocador, coherencia repaso↔profundidad verificada.

---

## Lo que queda

### A. Portar la arquitectura al TEMPLATE de origen — fuera de este repo

Listo el prompt autocontenido en [`_porteo-template.md`](_porteo-template.md): se copia en una sesión abierta en el repo del template. El porteo en sí ocurre allá, no aquí. Cuando esté hecho, borra ese archivo y este.

### B. La conversación de las horas — decisión del usuario, pendiente

Este es el objetivo final, y se dejó para el final a propósito porque dependía de tener el modelo honesto cerrado (ya lo está). Contexto que conviene no perder:

- Con el modelo honesto (aprender + mantener), en el cálculo previo **nada cabía**: parcial −102 h, final −205 h, UNI −281 h (~590 h de déficit acumulado). Opciones sobre la mesa, **aún sin decidir**: retirar cursos del semestre, no reestudiar la base a nivel alto, aceptar jalar algo, o mover metas.
- Todos esos números descansan sobre supuestos **sin medir**: 1.5 h/sesión, 12 min/repaso, el reparto mecánico de `sesiones_por_nivel`, y las 35/15 h semanales que son promesas. **El checkpoint del 26 de julio de 2026** (10 días de historial real) es la primera medición de verdad.
- El usuario decidió **bajar el listón de la base ya** (a nivel `base` para el semestre) sin esperar al 26, porque el historial mide igual con cualquier listón y quemar la ventana de vacaciones al ritmo caro era el riesgo mayor.
- Los 5 temarios del semestre son **inferidos** (ningún sílabo del IV ciclo está publicado) y todos llevan la obligación de revisarse en la semana 1 de clases (10-14 agosto 2026) contra el sílabo real.

Cuando A y B estén resueltos: borra este archivo, quita el aviso de migración del `README.md` y de `CLAUDE.md`, y el `README.md` vuelve a ser la única verdad sin coletillas.
