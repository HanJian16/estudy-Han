# Guía de herramientas de estudio

> Este archivo describe las herramientas externas que el agente puede recomendar al usuario. Cada entrada tiene una **fecha de última verificación** — si un agente con capacidad de web (WebFetch/WebSearch) detecta que la información está desactualizada, debe proponer al usuario actualizarla.

---

## Protocolo para el agente

**Cuando el usuario pregunte por una herramienta que no conoce:**
1. Busca la herramienta en este archivo.
2. Léele al usuario la descripción, el "¿Qué es?", el "¿Cómo se usa?" y el ejemplo concreto.
3. Si tiene URL, dásela y avísale que la abra en el navegador.

**Cuando el usuario diga que una herramienta cambió, ya no existe, o algo no coincide:**
1. Si tienes capacidad de web (WebFetch/WebSearch), verifica y actualiza la sección de esa herramienta. Actualiza también el campo `Última verificación`.
2. Si no tienes capacidad de web, avísale al usuario y sugiérele una alternativa mencionada en el mismo bloque ("Alternativas si esta falla").
3. Registra el cambio brevemente al final del archivo en la sección **Bitácora de cambios**.

**Cuando el setup inicial genere un curso nuevo:**
1. Revisa este archivo para identificar herramientas relevantes al área del curso.
2. Llena `herramientas_recomendadas` en el nuevo `temario.json` con las herramientas apropiadas (nombre, url, uso).
3. Si el área del curso no tiene herramientas listadas aquí, pregunta al usuario o busca sugerencias estándar y **añádelas a este archivo** en la sección correspondiente.

---

## Matemáticas y ciencias exactas

### WolframAlpha
- **URL:** https://www.wolframalpha.com
- **Última verificación:** 2026-07-01
- **¿Qué es?** Un motor de conocimiento computacional. Le escribes cualquier pregunta matemática, científica o factual en lenguaje natural (o notación matemática) y te devuelve la respuesta calculada, con pasos si tienes cuenta gratuita/pro. No es un buscador — resuelve, no lista páginas.
- **¿Cómo se usa?**
  1. Abre la URL.
  2. En la barra de búsqueda grande escribe tu problema. Puedes usar notación tipo `x^2` para exponentes, `sqrt(x)` para raíz, `integrate x^2 dx`, `solve 3x + 5 = 20`, etc.
  3. Enter → te muestra el resultado, la gráfica si aplica, formas alternativas.
  4. Con cuenta gratuita puedes ver algunos pasos; los pasos completos requieren cuenta "Pro" de pago.
- **Ejemplo concreto:** Escribes `solve x^2 - 5x + 6 = 0` → te muestra que x=2 y x=3, y grafica la parábola.
- **Bueno para:** verificar respuestas, ver gráficas rápidas, resolver ecuaciones que quieres corroborar, sistemas de ecuaciones.
- **Menos bueno para:** aprender el paso a paso sin pagar Pro (para eso Symbolab es mejor gratis).
- **Alternativas si esta falla:** Symbolab, Photomath (móvil), Microsoft Math Solver.

### Symbolab
- **URL:** https://www.symbolab.com
- **Última verificación:** 2026-07-01
- **¿Qué es?** Una calculadora simbólica online enfocada en mostrar el paso a paso de la resolución. Especialmente útil cuando te trabaste en un ejercicio y necesitas ver dónde te equivocaste.
- **¿Cómo se usa?**
  1. Abre la URL.
  2. Elige el tipo de operación en el menú lateral (Álgebra, Cálculo, Trigonometría, etc.) o pega la expresión directamente.
  3. Enter → te muestra la solución final y desplegable con "Show steps" para ver cada paso.
  4. En 2026 la mayoría de pasos son gratis; algunos módulos avanzados requieren cuenta premium.
- **Ejemplo concreto:** Pegas `factor(x^2 - 5x + 6)` → te muestra `(x-2)(x-3)` con los pasos: identificar que buscas dos números que sumen -5 y multipliquen 6, encontrar -2 y -3, escribir la factorización.
- **Bueno para:** ver el proceso completo cuando no entiendes cómo llegar del punto A al B.
- **Menos bueno para:** hacer trampa — si copias sin entender los pasos, no aprendes. Úsalo después de intentar, no antes.
- **Alternativas si esta falla:** WolframAlpha (con cuenta), Photomath, Microsoft Math Solver.

### Desmos (calculadora gráfica)
- **URL:** https://www.desmos.com/calculator
- **Última verificación:** 2026-07-01
- **¿Qué es?** Una calculadora gráfica web, gratuita, muy intuitiva. Escribes una función y la grafica en tiempo real. Se puede manipular con sliders (variables que arrastras) para ver cómo cambia la gráfica.
- **¿Cómo se usa?**
  1. Abre la URL.
  2. En el panel izquierdo escribe la función, ej: `y = x^2 - 3x + 2` o `f(x) = sin(x)`.
  3. La gráfica aparece automáticamente a la derecha.
  4. Para agregar sliders: escribe algo como `y = a*x + b` y Desmos te ofrece crear los sliders para `a` y `b`. Los mueves y ves cómo cambia la recta.
  5. Puedes marcar puntos con `(2, 4)`, restringir dominios, etc.
- **Ejemplo concreto:** Para visualizar el efecto del discriminante en una cuadrática: escribes `y = x^2 + b*x + c`, creas sliders de `b` y `c`, y ves cómo la parábola cruza el eje X (2 raíces, 1 raíz doble o ninguna).
- **Bueno para:** intuición visual de funciones, entender transformaciones (desplazamientos, escalados), ver raíces geométricamente.
- **Menos bueno para:** cálculos numéricos exactos (usa WolframAlpha para eso).
- **Alternativas si esta falla:** GeoGebra Graphing Calculator, Google en modo `x^2 + 3x - 2` (Google grafica simple).

### GeoGebra
- **URL:** https://www.geogebra.org
- **Última verificación:** 2026-07-01
- **¿Qué es?** Suite matemática con múltiples calculadoras: geometría (compás y regla virtual), gráficas 2D/3D, CAS (cálculo simbólico), álgebra. Especialmente fuerte para geometría plana y del espacio.
- **¿Cómo se usa?**
  1. Abre la URL y elige la calculadora apropiada:
     - **Geometry** para figuras planas (triángulos, círculos, construcciones con regla y compás).
     - **Graphing** para funciones (similar a Desmos).
     - **3D Calculator** para superficies y sólidos.
     - **CAS** para álgebra simbólica.
  2. Usa las herramientas del panel para dibujar puntos, rectas, círculos, medir ángulos, etc.
  3. Puedes exportar la construcción o compartirla.
- **Ejemplo concreto:** Para verificar que las medianas de un triángulo se cortan en un punto (baricentro): abre Geometry, dibuja un triángulo, usa la herramienta "punto medio" en cada lado, dibuja los segmentos del vértice opuesto al punto medio → verás que los 3 segmentos se cruzan en un punto.
- **Bueno para:** geometría clásica, construcciones formales, visualización 3D.
- **Menos bueno para:** cálculo simbólico complejo (Wolfram lo hace mejor).
- **Alternativas si esta falla:** Desmos Geometry (más simple), figuras a mano en cuaderno con regla y compás físicos.

### PhET (simulaciones interactivas)
- **URL:** https://phet.colorado.edu
- **Última verificación:** 2026-07-01
- **¿Qué es?** Proyecto de la Universidad de Colorado Boulder con simulaciones interactivas gratuitas de física, química, matemáticas, biología y ciencias de la Tierra. Cada simulación es una mini-aplicación web donde manipulas variables y observas efectos.
- **¿Cómo se usa?**
  1. Abre la URL, elige "Simulations" en el menú.
  2. Filtra por área (Physics, Chemistry, etc.) y nivel.
  3. Abre una simulación (correr en el navegador, algunas requieren Java/Flash antiguo — las nuevas son HTML5).
  4. Manipula sliders y controles, observa resultados.
- **Ejemplo concreto:** Para entender caída libre, abre "Projectile Motion", ajusta ángulo y velocidad inicial, dispara y observa la trayectoria. Puedes activar vectores para ver cómo la velocidad cambia con el tiempo.
- **Bueno para:** intuición conceptual en física clásica y química, ideal para armar experimentos mentales.
- **Menos bueno para:** cálculo numérico preciso, ejercicios de examen.
- **Alternativas si esta falla:** applets de HyperPhysics, simulaciones específicas por tema.

---

## Programación

### El propio Claude Code (u otro agente con ejecución)
- **URL:** local
- **Última verificación:** 2026-07-01
- **¿Qué es?** Si el agente tiene la capacidad de ejecutar código (Claude Code sí; muchos otros también), el propio entorno de estudio es la mejor herramienta. Se puede crear archivos, correr scripts, ejecutar tests, ver output.
- **¿Cómo se usa?** Pídele al agente que cree un archivo, escriba código, lo ejecute y muestre resultados. Para tests: escribe tests, corre `pytest` (Python), `npm test` (JS), etc.
- **Ejemplo concreto:** "Escribe una función que verifique si un número es primo y agregále 3 tests." → el agente crea el archivo, lo corre, valida.
- **Bueno para:** aprender programación de forma activa, verificar hipótesis instantáneamente.
- **Menos bueno para:** aprender teoría desconectada del código (para eso libros).

### Compiler Explorer (godbolt.org)
- **URL:** https://godbolt.org
- **Última verificación:** 2026-07-01
- **¿Qué es?** Herramienta web que muestra en tiempo real el assembly que genera tu código en varios compiladores y arquitecturas. Útil para entender qué hace realmente tu código de alto nivel.
- **¿Cómo se usa?** Pegas C/C++/Rust/etc a la izquierda, eliges compilador y flags, ves el assembly a la derecha con colores que ligan cada línea de origen con su bloque de assembly.
- **Bueno para:** cursos de arquitectura de computadoras, optimización, entender abstracciones de lenguaje.
- **Alternativas si esta falla:** flags de tu compilador local (`gcc -S`).

---

## Ciberseguridad

### TryHackMe
- **URL:** https://tryhackme.com
- **Última verificación:** 2026-07-01
- **¿Qué es?** Plataforma de aprendizaje de ciberseguridad basada en salas ("rooms") con máquinas virtuales vulnerables. Estructurado como caminos guiados (Junior Penetration Tester, Web Fundamentals, etc.). Nivel principiante-intermedio.
- **¿Cómo se usa?** Creas cuenta gratuita, eliges una sala o path, cada sala te da una VM temporal en el navegador (o te conectas por OpenVPN desde tu Kali local) y una guía paso a paso con preguntas. Muchas salas son gratis; el contenido avanzado es de pago.
- **Bueno para:** empezar en pentesting, aprender herramientas concretas (nmap, gobuster, sqlmap, metasploit).
- **Alternativas:** HackTheBox (más difícil), OverTheWire (bandit), portswigger.net (para web).

### HackTheBox
- **URL:** https://www.hackthebox.com
- **Última verificación:** 2026-07-01
- **¿Qué es?** Plataforma similar a TryHackMe pero orientada a nivel intermedio-avanzado. Menos hand-holding: te dan una máquina y hay que "romperla" (obtener acceso root/administrator). Community rank por dificultad.
- **¿Cómo se usa?** Suscripción (hay tier gratuito limitado). Te conectas por VPN a la red HTB, escaneas la máquina objetivo, explotas vulnerabilidades, obtienes user y root flags. Foros y writeups (después de retirar la máquina) para aprender.
- **Bueno para:** después de dominar TryHackMe, para prepararse a certificaciones como OSCP.

---

## Música (teoría)

### LilyPond
- **URL:** https://lilypond.org
- **Última verificación:** 2026-07-01
- **¿Qué es?** Un lenguaje de tipografía musical: escribes texto con notación específica y compila un PDF con la partitura profesional. Es a la música lo que LaTeX es a las matemáticas.
- **¿Cómo se usa?**
  1. Instala LilyPond (multiplataforma, disponible en su web).
  2. Escribe un archivo `.ly` con la partitura, ej: `\relative c' { c d e f g a b c }`.
  3. Compila con `lilypond archivo.ly` → obtienes un PDF con la partitura y opcionalmente un MIDI.
- **Bueno para:** aprender notación musical técnica, generar partituras editables por texto, compartir composiciones.
- **Menos bueno para:** si prefieres arrastrar notas visualmente (usa MuseScore).
- **Alternativas si esta falla:** MuseScore (GUI), Flat.io (web).

### MuseScore
- **URL:** https://musescore.org
- **Última verificación:** 2026-07-01
- **¿Qué es?** Editor de partituras gráfico y gratuito. Arrastras notas al pentagrama con el mouse o el teclado.
- **¿Cómo se usa?** Descarga la app, abre un pentagrama en blanco, elige compás y tonalidad, ingresa notas con el teclado o mouse. Reproduce con MIDI para escuchar.
- **Bueno para:** componer y escuchar, aprender ubicación de notas y armaduras.

---

## Idiomas

### Anki (repetición espaciada)
- **URL:** https://apps.ankiweb.net
- **Última verificación:** 2026-07-01
- **¿Qué es?** Sistema de tarjetas (flashcards) con algoritmo de repetición espaciada. Tú creas tarjetas frente-dorso, el algoritmo decide cuándo mostrártelas según qué tan bien las recordaste la última vez.
- **¿Cómo se usa?** Instala Anki (desktop gratis, móvil iOS de pago), crea un deck, agrega tarjetas. Estudia cada día — el algoritmo te muestra las que toca repasar.
- **Bueno para:** vocabulario de idiomas, definiciones, cualquier cosa que requiera memoria a largo plazo.
- **Nota:** este proyecto ya implementa repetición espaciada dentro del propio `glosario.json` y `dificultades.json`, así que Anki es complementario para vocabulario intensivo (idiomas, terminología médica, etc.).

### Forvo
- **URL:** https://forvo.com
- **Última verificación:** 2026-07-01
- **¿Qué es?** Diccionario colaborativo de pronunciaciones: buscas una palabra en cualquier idioma y escuchas grabaciones de hablantes nativos.
- **¿Cómo se usa?** Busca la palabra, elige idioma, reproduce las grabaciones (varias por palabra, distintos acentos).

---

## Bitácora de cambios

Registro de actualizaciones a este archivo. Formato: `YYYY-MM-DD — [agente] — [qué cambió]`.

- 2026-07-01 — creación inicial del archivo con herramientas base para matemáticas, programación, ciberseguridad, música e idiomas.
