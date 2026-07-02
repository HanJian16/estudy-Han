# Clase: Numeración y cambio de base

**Tema en temario:** `02-numeracion`
**Fecha:** 2026-07-02
**Número de sesión para este tema:** 1 (coincide con `sesiones_dedicadas` en `progreso.json`)

## Objetivo de esta sesión
Cubrir los tres objetivos de aprendizaje del tema: convertir numerales entre bases arbitrarias en ambos sentidos, aplicar descomposición polinómica para plantear ecuaciones con cifras, y resolver problemas de numerales capicúas.

## Recuperación activa inicial
Al arrancar, el usuario recordó correctamente el concepto de sistema posicional (el valor de una cifra depende de su posición) y la estructura de notación de base (cifras + subíndice de base). No recordaba con seguridad el algoritmo de cambio de base (solo la intuición de "dividir por la base"), pero lo reconstruyó él mismo al aplicarlo paso a paso guiado.

## Resumen / apuntes
- Sistema posicional: el valor de cada cifra es cifra x base^posición (posición 0 = unidades, contando desde la derecha).
- Cifra máxima válida en cualquier base b: b-1 (igual que en base 10 el máximo es 9).
- Base 10 -> otra base: divisiones sucesivas entre la base destino; el numeral final se arma con el último cociente + los restos en orden inverso.
- Otra base -> base 10: descomposición polinómica directa (sumar cifra x base^posición).
- Base A -> Base B (ninguna es 10): primero A -> base 10 (descomposición polinómica), luego base 10 -> B (divisiones sucesivas).
- Numeral de 2 cifras "ab" en base 10 = 10a + b. Numeral capicúa de 3 cifras "aba" = 101a + 10b.
- En problemas de conteo con cifras representadas por letras distintas (a, b), no hay restricción de que sean valores distintos salvo que el enunciado lo diga.

## Ejercicios trabajados
> No se resume ni parafrasea la respuesta del usuario. Cita textual + anotación del agente.

1. **Enunciado:** Convierte 91 (en base 10) a base 5.
   **Respuesta del usuario (textual):** "sale 331 en base 5, probe con la descomposición polinómica (3x5^2 + 3x5^1 + 1X5^0) y si cuadra (también lo comprobé con WolframAlpha)"
   **Anotación del agente:** Correcto y verificado por dos vías (autoverificación con descomposición polinómica + WolframAlpha). Aplicó bien el algoritmo de divisiones sucesivas que había reconstruido en la recuperación activa inicial.

2. **Enunciado:** Convierte 2536 (en base 7) a base 10.
   **Respuesta del usuario (textual):** "me salió 958 a mano y lo comprobé en wolframAlpha"
   **Anotación del agente:** Correcto (958). Primer uso de descomposición polinómica en sentido "otra base -> base 10", sin errores.

3. **Enunciado:** En un numeral de dos cifras, la cifra de las decenas es el doble de la cifra de las unidades. Si al invertir el orden de las cifras el número disminuye en 27, halla el numeral original.
   **Respuesta del usuario (textual, planteamiento):** "mi planteación fue 2x*10+x*1-x*10-2x*1=27" (captura de WolframAlpha) — luego, al preguntársele qué representaba x: "cuando definí la ecuación tome a X como la unidad y 2x como la decena, y le resté la inversa que sería a x como decena y 2x como unidad, plantee la ecuación igualando a la diferencia, que sería 27" — y finalmente: "el numeral de dos cifras es 63 y su inversa 36, restando quedan 27"
   **Anotación del agente:** Álgebra correcta desde el inicio (9x=27, x=3), pero inicialmente dio "el número era 3" como respuesta, confundiendo la variable auxiliar con el numeral pedido. Corrigió solo tras preguntas guía (ver `dif-02-01`), llegando a 63 correctamente.

4. **Enunciado:** Un numeral capicúa de 3 cifras tiene como cifra de centenas el 6, y la suma de todas sus cifras es 15. Halla el numeral.
   **Respuesta del usuario (textual):** "el numeral es 636"
   **Anotación del agente:** Correcto y directo, sin errores ni necesidad de corrección. Buena señal de que la descomposición polinómica para capicúas ya estaba asentada.

5. **Enunciado (estilo UNI):** Si el numeral "24" en base n representa el mismo valor que 20 en base 10, halla el valor de n.
   **Respuesta del usuario (textual):** "mi planteación fue 2n+4 = 20, sale que n es 8 por lo que sería 24 en base 8 igual a 20 en base 10"
   **Anotación del agente:** Correcto, sin errores.

6. **Enunciado (estilo UNI):** Un numeral de tres cifras cumple: la cifra de las centenas excede en 2 a la cifra de las unidades, y la cifra de las decenas es el triple de la cifra de las unidades. Si la suma de sus tres cifras es 17, halla el numeral.
   **Respuesta del usuario (textual):** "mi planteamiento fue x+2+3x+x=17, dandome que x es igual a 3 por lo que el numeral que es (x+2)(3x)x = 593"
   **Anotación del agente:** Correcto, y esta vez sí tradujo la variable auxiliar de vuelta al numeral completo sin necesidad de pregunta guía — señal de que `dif-02-01` quedó internalizada en la misma sesión.

7. **Enunciado (estilo UNI):** ¿Cuántos numerales capicúas de 4 cifras existen en total?
   **Respuesta del usuario (textual):** "pues en un numeral abba a puede tomar cualquier numero menos el 0 por lo que serían 9 opciones, y b podría tomar cualquier número menos el mismo que a por lo que tambien serían 9 opciones, como las otras dos cifras son copias sería un 9*9 y eso es 81 combinaciones" — tras pregunta guía con el ejemplo 1111: "Si tienes razón, me confundí al decir que b y a no podrían tener el mismo valor al ver letras diferentes, un juego psicológico ingenioso, entonces sería un 9*10 = 90"
   **Anotación del agente:** Error conceptual (ver `dif-02-02`), corregido de inmediato al contrastar con el caso concreto 1111. Buena capacidad de autocorrección cuando se le da un contraejemplo.

## Herramientas usadas
- WolframAlpha — verificación de conversiones de base y de la ecuación algebraica del ejercicio 3.
- Cuaderno físico — desarrollo a mano de todos los ejercicios antes de verificar.

## Dudas / puntos flojos
Ver `dif-02-01` a `dif-02-04` en `dificultades.json`. Resumen: (a) confundir variable auxiliar con respuesta final en ecuaciones de cifras, (b) asumir que cifras con letras distintas no pueden repetir valor, (c) calcular multiplicador posicional como notación de ceros en vez de potencia real, (d) usar una cifra igual a la base al inventar un ejemplo propio. Los cuatro se resolvieron dentro de la misma sesión, pero quedan registrados para repaso espaciado por si reaparecen.

## Feynman de cierre
**Consulta del agente (textual):** "explícame el tema de hoy — numeración y cambio de base — como si yo no supiera absolutamente nada del área. Usa tus propias palabras, sin tecnicismos que no puedas justificar, y cúbrelo todo: qué es un sistema posicional, cómo se convierte entre bases, y cómo se usa la descomposición polinómica para plantear ecuaciones con cifras."

**Respuesta del usuario (textual):** "la numeración es la posición de cada cifra en un número y el valor real que este representa en esa connotación (por ejemplo que un 1 en la posición de decenas equivale a 10) el multiplicador depende de la base en la que se encuentre un numeral (x7 si son las decenas de un numeral en base 7 o x200 si es una centena de un numeral en base 2). Para el cambio de base es sencillo, para cambiar un numeral de base 10 a cualquier otra base lo que se hace es una división continua del numeral y la base objetivo, hasta que el cociente que quede sea menor a la base, y luego se toma el ultimo cociente y los residuos en orden inverso, y para pasar de un numeral de cualquier otra base a base 10 se hace con descomposición polinómica que es una multiplicación del numeral correspondiente con su base elevado a la posición en la que se encuentra (por ejemplo 57 en base 7 es 5*7^1 + 7*7^0) esa suma te entrega el numeral en base 10, obviamente si quieres pasar de un numeral en base 5 por ejemplo a su respectivo en base 8, primero debes pasar el numeral a base 10 con descomposición polinómica y luego pasarlo de base 10 a base 8 con división repetida." — tras dos preguntas de corrección: "Ah tienes razón, no coincide, el valor de una centena en base 2 es un x4" y "tambien tienes razón no debe existir el numeral 57 en base 7, ese número debería ser 60 en base 7 ya que no puede existir la cifra 7 valida para un numeral en base 7 ya que su tope es la cifra 6 (uno menos que la base)"

**Anotación del agente:** Sólido en el mecanismo de conversión (ambos sentidos) y, especialmente, en el proceso base A -> base B vía base 10 — lo articuló sin que se le diera esa regla explícitamente, es una generalización propia. Dos lagunas detectadas y corregidas en el momento: (1) calculó el multiplicador posicional de una centena en base 2 copiando la convención de escritura de base 10 ("agregar 00") en vez de aplicar la potencia real (2^2=4); (2) usó una cifra igual a la base (7 en base 7) en su propio ejemplo, violando la regla de cifra máxima que él mismo había enunciado bien antes en la sesión (ver `dif-02-03` y `dif-02-04`). Ambas se resolvieron con una sola pregunta guía cada una, señal de que el hueco es más de "reflejo automático" que de comprensión real — vale la pena un repaso rápido en la próxima sesión de este tema antes de dar por completado el criterio de dominio.

## Próximos pasos
Continuar `02-numeracion` en la próxima sesión (quedan ~35% del tema y sesiones_estimadas permite una sesión más): repasar brevemente `dif-02-03` y `dif-02-04` (multiplicador posicional real vs. notación de ceros; cifra máxima al generar ejemplos propios) antes de dar el tema por completado, y reforzar con más ejercicios variados de nivel examen hasta llegar con confianza al criterio de dominio (8/10 sin ayuda).
