# Instrucción de arranque para Claude Code

> Este archivo se carga automáticamente al iniciar la sesión. Su único propósito es redirigir al documento principal para no duplicar contenido.

## ⚠️ LÍMITES REALES DEL PROYECTO — LEE EL AVISO DEL README

El `README.md` empieza con un bloque titulado **"LÍMITES REALES DE ESTE PROYECTO"**. Léelo entero antes de actuar. Resume tres cosas que este proyecto NO puede garantizar: (1) nada obliga al agente a seguir estos protocolos —cargarse no es ejecutarse—; (2) los archivos de rastro demuestran que un paso ocurrió, no que se hiciera bien, porque los escribe el propio agente; (3) escribir una regla no cambia el comportamiento del agente, y está demostrado dos veces en este repo.

**Consecuencia práctica: lo que se pueda ejecutar, ejecútalo — no lo leas.** El 2026-07-18, cinco de cinco veces que se corrió un script aparecieron fallos que llevaban semanas escondidos; leyendo no apareció ninguno. Si afirmas haber hecho una comprobación, pega su salida real.

---

## 🚧 MIGRACIÓN INTERNA CERRADA (2026-07-16 → completada)

El núcleo del proyecto se rediseñó al modelo de dos dimensiones (profundidad + durabilidad) y **ese rediseño ya está completo**: el `README.md` NO se contradice a sí mismo y es la única fuente de verdad viva. Puedes seguirlo directamente.

Queda [`PENDIENTE.md`](PENDIENTE.md) por **una sola cosa** fuera del flujo normal: portar la arquitectura al repo del template (prompt en `_porteo-template.md`). Léelo si vas a trabajar en eso; para una sesión de estudio normal, el `README.md` basta.

La coordinación con la agenda externa ("Ángela") vive en [`_protocolos/coordinacion-agenda.md`](_protocolos/coordinacion-agenda.md) y la dispara el **PASO 4.5** del `README.md`. No necesitas leerla salvo que toque un sync.

---

**El documento principal y única fuente de verdad de este proyecto es [`README.md`](README.md).**

Antes de responder cualquier cosa al usuario:

0. **Lee [`_protocolos/capas.md`](_protocolos/capas.md) y aplícalo — en CADA mensaje, no solo al arrancar.** Desglosa el mensaje en acciones, verifica las restricciones vigentes, y relee los protocolos de la capa de cada acción antes de ejecutarla. Es el PASO 0 del algoritmo. No lo saltes por creer que ya te sabes los protocolos: esa creencia es exactamente el fallo que este paso existe para atrapar.
0b. **Lee `PENDIENTE.md` si existe** (ver arriba).
1. **Lee `README.md` por completo.** Contiene el algoritmo de arranque obligatorio, los protocolos, los métodos de estudio y las reglas de calidad. No confíes en tu memoria de conversación ni en resúmenes: léelo cada sesión.
2. **Ejecuta el "Algoritmo de arranque" del `README.md` paso a paso**, empezando por verificar si existe `objetivo.json`. Los pasos son estrictamente secuenciales.
3. **Sigue todas las "Reglas irrompibles"** definidas en ese archivo.

No hay reglas adicionales aquí: todo vive en `README.md`. Si alguna vez este archivo y el `README.md` se contradicen, **manda el `README.md`**.
