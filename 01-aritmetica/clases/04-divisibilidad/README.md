# Clase: Divisibilidad

**Tema en temario:** `04-divisibilidad`
**Fecha:** 2026-07-05
**Número de sesión para este tema:** 1

## Objetivo de esta sesión
- Repasar la base de `03-cuatro-operaciones` (D=d·q+r) aplicada a divisibilidad.
- Introducir la notación de múltiplos/congruencia usada en problemas tipo UNI.
- Cubrir criterios de divisibilidad que faltan (4, 8, 11) y dejar planteado el método combinado de 7-11-13 para la siguiente sesión.

## Recuperación activa inicial
Sin apoyo de apuntes, el usuario recordó:
- **D = d·q + r:** definición correcta de cada letra y la relación con el residuo por defecto. Sin errores de notación d/q (a diferencia de `dif-03-02`, ya resuelta).
- **Múltiplo de 7:** lo definió correctamente como "es 7 por algo" / divisible en 7 partes iguales.
- **Criterios que ya domina de secundaria:** por 2 (última cifra par), por 5 (última cifra 0 o 5), por 3 (suma de cifras múltiplo de 3), por 9 (suma de cifras múltiplo de 9, pero no de 3), por 6 (cumple 2 y 3 a la vez). No recordaba 4, 8, 7, 11, 13.
- Base sólida para construir sobre ella; no hay huecos conceptuales de temas previos.

## Resumen / apuntes

### Bloque 1 — Por qué funciona el criterio de 3 y 9 (derivación propia)

En vez de memorizar el criterio, se derivó con la descomposición polinómica ya dominada (`02-numeración`). Para un número de 3 cifras "abc":

$$abc = 100a + 10b + c$$

Dato clave (hallado por el usuario dividiendo 10, 100, 1000 entre 9): toda potencia de 10 deja **residuo 1** al dividirla entre 9:

$$100 = 9\times11 + 1, \qquad 10 = 9\times1 + 1$$

Sustituyendo:

$$100a + 10b + c = (9\times11 + 1)a + (9\times1+1)b + c = (99a + 9b) + (a+b+c)$$

Como $99a+9b$ es múltiplo de 9 sin importar los valores de $a,b$, la divisibilidad de todo el número por 9 depende **únicamente** de $a+b+c$ (la suma de cifras). El mismo argumento aplica a 3, porque 9 es múltiplo de 3, así que $99a+9b$ también es múltiplo de 3.

> Nota de proceso: el usuario dedujo esto solo a partir de tres preguntas guía (calcular residuos de 10, 100, 1000 entre 9; sustituir; agrupar). No se le dio la regla directa.

### Bloque 2 — Notación de múltiplos y congruencia (pendiente de cerrar en el chat por límite de LaTeX)

Notación estándar usada en libros peruanos (Lumbreras/ADUNI) para expresar "N es un múltiplo de $k$":

$$N = \dot{k} \quad \text{o equivalente} \quad N = k\alpha \;\; (\alpha \in \mathbb{Z})$$

Se lee "N es $k$" (con el puntito arriba indicando "múltiplo de"). Cuando no es exacto y sobra un residuo $r$:

$$N = \dot{k} + r$$

Se lee "N es $k$ más $r$". Es la misma idea que $D = d\cdot q + r$ de `03-cuatro-operaciones`, solo que aquí no nos importa el valor exacto del cociente — solo que existe un entero que lo hace múltiplo — por eso se usa una letra genérica ($\alpha$, $k$, etc.) en vez de $q$.

**Ejemplo:** "Un número que es $\dot{7}+3$" significa que al dividirlo entre 7 sobra 3 (ej. 10, 17, 24, ...).

### Bloque 3 — Criterio de 4 y 8 (derivación propia)

Mismo tipo de razonamiento que 9/3, pero mirando residuos de **potencias de 10 respecto a 4 y 8**:

- $100 \div 4 = 25$, residuo **0** → 100 es múltiplo exacto de 4. Por descomposición polinómica, cualquier cifra de centenas o más forma un múltiplo de 100, y por lo tanto de 4. Conclusión: la divisibilidad por 4 depende **solo de las últimas 2 cifras**.
- $100 \div 8$: **no** es exacto ($8\times12=96$, residuo 4) → el criterio de 4 NO aplica a 8 con 2 cifras. Se probó con $1000 \div 8 = 125$, residuo **0** → 1000 sí es múltiplo exacto de 8. Conclusión: la divisibilidad por 8 depende de las **últimas 3 cifras**.

> Nota de proceso: el usuario intentó primero generalizar el criterio de 4 directamente a 8 con 2 cifras (asumiendo $100\div8=15$, error aritmético). Se autocorrigió al verificar $8\times15=120\ne100$, y luego derivó solo el criterio correcto probando con 1000.

### Bloque 4 — Criterio de 11 (suma alternada)

Basado en el residuo por exceso ya conocido de `03-cuatro-operaciones`: $10 \div 11$ tiene residuo por exceso **1** (le "falta 1" para el siguiente múltiplo de 11), es decir $10 \equiv -1 \pmod{11}$.

Como cada potencia de 10 es el producto de la anterior por 10, el signo se alterna en cada posición:

$$10^0\equiv+1,\quad 10^1\equiv-1,\quad 10^2\equiv+1,\quad 10^3\equiv-1,\ \dots \pmod{11}$$

**Criterio:** agrupar las cifras alternadamente empezando por las unidades con signo +, sumar cada grupo con su signo. Si el resultado es múltiplo de 11 (incluyendo 0), el número lo es.

$$N = \dots + d_3(1000) + d_2(100) + d_1(10) + d_0 \;\equiv\; (d_0+d_2+d_4+\dots) - (d_1+d_3+d_5+\dots) \pmod{11}$$

**Ejemplo:** 9,548 → cifras (de derecha a izq., posición 0,1,2,3): 8,4,5,9. Suma posiciones pares (0,2): 8+5=13. Suma posiciones impares (1,3): 4+9=13. Diferencia: 13−13=0 → múltiplo de 11 (9548 = 11×868).

### Bloque 5 — Pendiente para próxima sesión
- Método combinado de 7, 11 y 13 (bloques de 3 cifras), aprovechando que $1001 = 7\times11\times13$ y que $1000 \equiv -1$ respecto a los tres simultáneamente.

## Ejercicios trabajados
> No se resume ni parafrasea la respuesta del usuario. Cita textual + anotación del agente.

1. **Enunciado:** Halla el menor valor de la cifra $x$ para que el numeral $385x$ sea múltiplo de 4.
   **Respuesta del usuario (textual):** "x= 2, el mínimo múltiplo de 4 con decena 5 es 52"
   **Anotación del agente:** Correcto y directo (52 = 4×13). Aplicó bien el criterio recién derivado (solo importan las 2 últimas cifras) sin errores.

2. **Enunciado:** Halla el valor de la cifra $b$ para que el numeral $6b3$ sea múltiplo de 11.
   **Respuesta del usuario (textual):** "Eso nos deja 6-b+3 es un múltiplo de 11, osea 9-b es múltiplo de 11, entonces b = 9"
   **Anotación del agente:** Correcto. Planteó bien la suma alternada (posiciones pares 6+3, posición impar b) y descartó implícitamente los casos fuera de rango de un dígito (9-b=±11 no daría dígito válido). Buen manejo del criterio de 11 en su primer uso.

3. **Enunciado:** Al dividir un número N entre 9, el residuo es el mayor posible. Si además N es múltiplo de 5 y está entre 90 y 130, halla N.
   **Respuesta del usuario (textual):** "N = 9() + 8 / N = 5() / 90<= N <= 130 / entonces tenemos que un múltiplo de 9 + 8 = multiplo de 5, como los múltipos de 5 terminan en 0 o 5 es más facil ponerlo como. multiplo de 9 = multiplo de 5 - 8 / entonces serían multiplo de 9 = ...2 o ...7 / pues un múltiplo de 5 menos 8 solo puede terminar en 2 o 7. / Los múltiplos de 9 entre 90 y 130 son: 90, 99, 108, 117, 126 / el único que cuadra es 117 = ...7 / entonces N = 117"
   **Anotación del agente:** Planteamiento y búsqueda del múltiplo de 9 correctos (117), pero error final: dio como respuesta el múltiplo de 9 (117) en vez de sumarle el residuo 8 para obtener N, pese a tener la ecuación N=9()+8 escrita arriba. Mismo patrón que `dif-02-01` y `dif-03-03` (confundir la variable auxiliar con la respuesta final que pide el problema), ahora en un tema distinto (divisibilidad). Se le señaló releer su propia ecuación y corrigió de inmediato a N=125.
   **Respuesta corregida (textual):** "Tienes razón, se me olvidó sumar el residuo de 8, entonces sería 125, el número que es múltiplo de 5 y multiplo de 9 + 8"

4. **Enunciado:** Halla todos los valores posibles de la cifra $x$ para que el numeral $2x4$ sea divisible entre 6.
   **Respuesta del usuario (textual):** "2x4 = 6() = 2() y 3() / ya sabemos que es múltiplo de 2 por que su última cifra es 4, entonces nos centramos en la multiplicidad de 3. / 2+x+4 = 3() / 6+x = 3() / entonces x = {0, 3, 6, 9}"
   **Anotación del agente:** Correcto y completo (los 4 valores). Descompuso bien "divisible entre 6" como "divisible entre 2 y 3 a la vez", reconoció que la condición de 2 ya se cumple sin depender de x, y encontró el conjunto completo de soluciones (no solo una).

## Herramientas usadas
- Cuaderno físico — cálculo de residuos de 10, 100, 1000 entre 9, 4, 8 y 11 a mano; desarrollo de los 4 ejercicios.

## Dudas / puntos flojos
- **Patrón recurrente (no nuevo, pero reaparece):** confundir la variable auxiliar/intermedia con la respuesta final que pide el problema (ejercicio 3 de hoy). Ya visto en `dif-02-01` (numeración) y `dif-03-03` (cuatro operaciones), ambas marcadas resueltas en su momento — pero el patrón reapareció en un tema nuevo (divisibilidad), lo que sugiere que la estrategia de "etiquetar cada dato" (de `dif-03-03`) no se extendió aún a etiquetar explícitamente **qué representa la incógnita que se resuelve primero** vs. lo que pide la pregunta final. Se registra como `dif-04-01`.
- Cálculo aritmético puntual (100÷8, ejercicio de apuntes, no de la lista numerada): confundió brevemente el criterio de 4 con el de 8, corregido al instante verificando 8×15≠100. No se registra como dificultad nueva (desliz de un solo paso, autocorregido con una verificación directa).

## Feynman de cierre
**Consulta del agente (textual):** "Explícame como si se lo enseñaras a alguien que nunca vio este tema: ¿por qué el criterio de divisibilidad por 9 (o por 3) consiste en sumar las cifras, y por qué el de 4 y 8 consiste en mirar solo las últimas cifras? No repitas la receta de memoria — explica el porqué usando lo que dedujimos hoy (los residuos de las potencias de 10). Puedes usar un ejemplo propio."

**Respuesta del usuario (textual):** "Por que al hacer descomposiciónes polinómicas (110a + 10b + c), podemos encontrar algunos parámetros, por ejemplo para el caso de 3 y 9, al hacerlo y agrupar la descomposición por un múltiplo de 9, nos queda como residuo la suma de las cifras (a+b+c), entonces es dependiendo de esta suma que veremos si se completa para ser un múltiplo de 3 y 9, por otro lado para 8 y 4, a partir de 1000 y 100 respectivamente todos los valores de la descomposición se vuelven múltiplos de 8 y 4, (1000/8 es exacto y 100/4 también), entonces solo dependen de las últimas partes de la descomposición (100a + 10b + c para multiplos de 8 y 10b + c para multiplos de 4)"

**Anotación del agente:** Explicación sólida y completa: conecta correctamente la descomposición polinómica con la agrupación en múltiplos conocidos para 9/3, y distingue bien el alcance de 4 (últimas 2 cifras, 10b+c) del de 8 (últimas 3 cifras, 100a+10b+c) justificándolo con la exactitud de 100÷4 y 1000÷8. Único desliz: escribió "110a" en vez de "100a" al inicio (typo de tipeo, no conceptual — el resto de la explicación usa 100a correctamente). No se menciona el criterio de 11, pero no se le pidió explícitamente en esta consigna. Tema recomendado para reforzar en la siguiente sesión con Feynman: el criterio de 11 y su relación con el residuo por exceso.

## Próximos pasos
Sesión 2 de este tema (04-divisibilidad): cubrir el método combinado de 7, 11 y 13 (bloques de 3 cifras), reforzar `dif-04-01` (etiquetar qué representa la incógnita vs. la respuesta final) con 2-3 problemas de planteo con residuos, y empezar problemas de conteo de múltiplos en intervalos (inclusión-exclusión) para acercarse al criterio de dominio (7/10 problemas nivel UNI).
