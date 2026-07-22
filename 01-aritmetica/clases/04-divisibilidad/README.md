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

---

# Sesión 2

**Fecha:** 2026-07-08
**Número de sesión para este tema:** 2

## Objetivo de esta sesión
- Cubrir el método combinado de divisibilidad por 7, 11 y 13 (bloques de 3 cifras).
- Reforzar `dif-04-01` con problemas de planteo con residuos.
- Iniciar conteo de múltiplos en un intervalo e inclusión-exclusión aplicada a múltiplos.
- Cerrar el criterio de dominio del tema.

## Recuperación activa inicial
Sin apoyo de apuntes, el usuario recordó correctamente los tres criterios de la sesión 1 (por qué funciona el de 9/3 vía descomposición polinómica, por qué 4 usa 2 cifras y 8 usa 3, y el criterio de 11 con signos alternados). Dos imprecisiones de lenguaje detectadas y autocorregidas en el momento:
- Dijo "últimas 3 cifras" para el criterio de 4 (típeo; sabía que eran 2, lo confirmó al repreguntarle).
- Dijo "posiciones pares negativo, impares positivo" para el criterio de 11 (invertido respecto a su propio cálculo de $10^2=+1$); se autocorrigió explicando que el criterio confiable es por exponente (0-indexado), no por "posición" contada desde 1, y agregó una versión rápida propia (empezar en unidades con +, alternando).

## Resumen / apuntes

### Bloque 5 — Método combinado de 7, 11 y 13 (derivación propia)
El usuario calculó que 1000 ÷ 7, ÷ 11 y ÷ 13 dan residuo por exceso 1 en los tres casos (confirmado: 1000=7×142+6 con exceso 1; 1000=11×90+10 con exceso 1; 1000=13×76+12 con exceso 1), y conectó esto con que 1001 = 7×11×13. Concluyó que 1000 se comporta como "-1" para los tres divisores a la vez, y que agrupando el numeral en bloques de 3 cifras desde la derecha (cada bloque como número completo) con signo alternado, se puede revisar la divisibilidad del numeral original a través de un número mucho más chico. Ver definición completa en `glosario.json` ("Método combinado de divisibilidad por 7, 11 y 13").

### Conteo de múltiplos en un intervalo
A partir de la fórmula ya conocida de conteo de progresión aritmética (`03-cuatro-operaciones`), se agregó la técnica de hallar el primer y último múltiplo de un intervalo usando residuo por exceso/defecto sobre los extremos. Ver `glosario.json` ("Hallar el primer/último múltiplo de un intervalo").

### Inclusión-exclusión aplicada a conteo de múltiplos
Se conectó con la fórmula de cardinalidad de `01-conjuntos` (ya sólida): contar múltiplos de a "o" de b en un intervalo es n(a)+n(b)-n(a∩b), donde n(a∩b) se cuenta como múltiplos del MCM(a,b) (en el caso trabajado, ambos coprimos, a×b).

### Combinación de dos congruencias por sustitución (introducción parcial)
Al preguntar el usuario por una forma más rápida que listar y filtrar, se introdujo la sustitución de una condición de múltiplo dentro de la otra. Funciona directo cuando el coeficiente de la incógnita se reduce a ±1 respecto al segundo módulo (caso con 7 y 8). Se intentó extender a un caso con coeficiente 4 (mód 9) y no se pudo despejar por división directa — requiere inverso modular, herramienta que depende de MCD/algoritmo de Euclides, no cubierto aún. Se resolvió ese caso por tanteo de los 9 residuos posibles como solución temporal. Técnica completa pendiente para `05-primos-mcd-mcm`. Ver `glosario.json` (nivel de dominio "reconoce", no "solido", intencionalmente).

## Ejercicios trabajados
> No se resume ni parafrasea la respuesta del usuario. Cita textual + anotación del agente.

1. **Enunciado:** ¿Es $2\,145\,678$ múltiplo de 7, de 11, de 13, de alguno, de ninguno? (agrupando en bloques de 3 cifras desde la derecha)
   **Respuesta del usuario (textual, primer intento):** "2-1-4-5=6=7=8 = 13, entonces es múltiplo de 13" (tras corrección de tipeo: "2 145 678 = 2 - 1 - 4 - 5 + 6 + 7 + 8 = 13")
   **Anotación del agente:** Error conceptual: trató cada dígito individual con signo propio en vez de cada bloque de 3 cifras como número completo (ignoró el valor posicional dentro del bloque, ej. tomó "1,4,5" en vez de "145"). Se le preguntó si $1+4+5$ vale lo mismo que $145$; se autocorrigió de inmediato.
   **Respuesta corregida (textual):** "Ahhh entiendo, claro, entonces sería: 2 - 100 - 40 - 5 + 600 + 70 + 8 = 535. Y no es divisible ni por 11, 7 o 13, ya que si divides 535 entre alguno de ellos no es exacto." Correcto: bloques 678(+), 145(-), 2(+) = 535, no exacto entre ninguno de los tres.

2. **Enunciado:** ¿Es $351\,463$ múltiplo de 7? ¿De 11? ¿De 13?
   **Respuesta del usuario (textual):** "-351 + 463 = 112 / 112/7 = 16 / 112/11 o 112/13 no es exacto. / así que solo es divisible por 7."
   **Anotación del agente:** Correcto y directo, sin ayuda. Aplicó bien el método ya corregido del ejercicio anterior.

3. **Enunciado:** ¿Cuántos múltiplos de 6 hay entre 40 y 250?
   **Respuesta del usuario (textual):** "Si divido 40/6 me da un resto de 4, el resto por exceso es de 2, si sumo eso a 40 me da el primer término múltiplo de 6 entre 40 y 250 que es 42. Haces algo similar para el último término, 250/6 da resto 4, entonces 250 - 4 = 246, ese es el último término múltiplo de 6 entre 40 y 250." Luego: "n = ((246-42)/6)+1 = 35, esa es la cantidad de múltiplos de 6 que hay en el intervalo"
   **Anotación del agente:** Correcto y sin ayuda en ambos pasos (hallar extremos y aplicar la fórmula).

4. **Enunciado:** ¿Cuántos números entre 1 y 150 son múltiplos de 3 o de 5 (al menos uno de los dos)?
   **Respuesta del usuario (textual):** "n(3) = ((150-3)/3)+1 = 50 / n(5) = ((150-5)/5)+1 = 30 / n(3&5) = ((150-15)/15)+1 = 10 / entonces por conjuntos... la unión de ambos, que sería = 50+30-10 = 70"
   **Anotación del agente:** Correcto y sin ayuda. Buena conexión espontánea con la fórmula de cardinalidad de `01-conjuntos`.

5. **Enunciado:** Un número N, al dividirlo entre 12, deja residuo 7. Además, N es múltiplo de 5. Si $150<N<220$, halla N.
   **Respuesta del usuario (textual, primer intento):** "N+7 = 12() / N = 5() ... entonces N = 192 -7 = 185."
   **Anotación del agente:** Error de signo en la ecuación inicial (N+7=12() en vez de N=12()+7), registrado como `dif-04-02`. Se le pidió comparar contra la definición ya sólida de notación de múltiplos.
   **Respuesta corregida (textual):** "Tienes razón, y es que se ve más sencillo si lo pongo en una ecuación 5() = 12() + 7, entonces para mantener la igualdad 5() - 7 = 12()... entonces no debe finalizar en ...7 o ...2, sino en ...3 o ...8 ... {144, 156, 168, 180, 192, 204} ... entonces el único número que cuadra es 168 = N-7 / N= 175." Correcto tras la corrección, con razonamiento completo y verificable.

6. **Enunciado:** Halla la suma de todos los valores que puede tomar la cifra $a$ para que el numeral $4a76$ sea divisible entre 12.
   **Respuesta del usuario (textual):** "4a76 = 12() = 4() y 3() / para ser múltiplo de 4 las dos últimas cifras deben ser múltiplos de 4, y 76 = 4*19, entonces cumple / 4 + a + 7 + 6 = 3() / a + 17 = 3() / a = {1, 4, 7} / Hallar la suma, 1+4+7= 12"
   **Anotación del agente:** Correcto y sin ayuda. Buena descomposición de "divisible entre 12" como "divisible entre 4 y entre 3 a la vez", reutilizando el criterio de 4 de la sesión 1 y el de 3 sin errores.

7. **Enunciado:** Un número N, al dividirlo entre 8, deja residuo 3. Además, N es múltiplo de 7. Si $100<N<200$, halla N.
   **Respuesta del usuario (textual):** "N = 8() + 3 / N = 7() ... del intervalo tomamos que el primer múltiplo de 7 sería 105... {105, 112,...,196} / De estos solo 147 cumple la propiedad para que al restarle 3 sea un múltiplo de 8 que es 144 / Entonces N = 147"
   **Anotación del agente:** Correcto y sin ayuda, planteando la ecuación con el signo correcto desde el inicio (sin repetir `dif-04-02`) y resolviendo con su método de listar y filtrar.

8. **Enunciado (práctica de la técnica de sustitución):** N es múltiplo de 5, y al dividirlo entre 9 deja residuo 4. Si $50<N<95$, halla N con la técnica de combinación de congruencias.
   **Respuesta del usuario (textual):** "5 = 9-4, entonces 5 debería comportarse como -4... 5k sería -4k, y esto dividido entre 9 daría residuo 4... me quedaría que k es igual a múltiplo de 9 (9m) sumado a 5/4, no a un número entero... no ví la forma de reemplazarlo correctamente."
   **Anotación del agente:** No fue un error del usuario sino una limitación de la técnica en este caso (coeficiente 4, no ±1) — requiere inverso modular, no cubierto aún. Se resolvió con ayuda del agente por tanteo de los 9 residuos posibles, obteniendo N=85. Detalle completo en `glosario.json`. Pendiente retomar con MCD/Euclides en `05-primos-mcd-mcm`.

## Herramientas usadas
- Cuaderno físico — cálculo de residuos y desarrollo de los 8 ejercicios.

## Dudas / puntos flojos
- `dif-04-02` (nueva, resuelta en la sesión): confundir el signo al traducir "residuo al dividir entre k" a una ecuación (N+r=k() en vez de N=k()+r). Autocorregida tras contrastar con la notación de múltiplos ya sólida; no reapareció en el ejercicio 7 inmediato siguiente.
- Combinación de dos congruencias con coeficiente distinto de ±1: no dominado, pendiente de forma intencional hasta contar con MCD/algoritmo de Euclides (`05-primos-mcd-mcm`). No se registra como dificultad porque es contenido fuera del alcance de este tema, no un hueco de lo que sí correspondía aprender hoy.

## Feynman de cierre
**Consulta del agente (textual):** "Explícame, como si se lo enseñaras a alguien que no sabe nada de este tema, el método combinado de divisibilidad por 7, 11 y 13 (el de agrupar en bloques de 3 cifras). No repitas la receta de memoria — explica el porqué, usando lo que dedujimos hoy sobre el comportamiento de 1000 respecto a esos tres divisores."

**Respuesta del usuario (textual):** "pues el comportamiento que tiene el número 1000 al dividirlos entre 7, 11 y 13 nos deja siempre de residuo 1, esto nos ayuda por que al hacer descomposición polinómica, lo que queremos es hacer una fórmula de residuos para saber el patrón que nos ayude a identificar rápidamente si son múltiplos o no de tal número. gracias a ello podemos hacer una modificación y tomar a 1000 = -1. Con esto se puede decir que cada 4 cifras (x1000) hay un cambio de positivo a negativo, así que se puede hacer una agrupación de 3 cifras empezando de derecha a izquierda, entonces si por ejemplo tenemos el numeral 2 097 813 podemos fácilmente agrupar en grupos (completos o incompletos) de 3, quedando (2) (097) (813) y hacer la sumatoria o resta con su respectivo signo => 2-97+813 = 718, y así tener un número más manejable para revisar si es múltiplo o no."

**Anotación del agente:** Explicación sólida y correcta, con ejemplo propio bien resuelto incluyendo un bloque incompleto (caso borde manejado sin errores). Única imprecisión menor de lenguaje: "cada 4 cifras hay un cambio" es una forma algo confusa de decir "a partir de la 4ta cifra empieza el siguiente bloque con signo opuesto" — no afecta la aplicación, que fue impecable. Tema registrado como sólido en glosario.

## Próximos pasos (registrado en su momento)
`04-divisibilidad` completado (criterio de dominio alcanzado: 6 de 8 problemas centrales del tema resueltos sin ayuda, los 2 restantes autocorregidos en el momento tras una pregunta guía, sin necesidad de que se les diera la respuesta). Siguiente sesión: iniciar `05-primos-mcd-mcm` (descomposición canónica, MCD, MCM, algoritmo de Euclides). Retomar ahí, como aplicación adicional, la técnica de combinación de congruencias con coeficiente distinto de ±1 usando el inverso modular una vez cubierto el MCD.

---

# Sesión 3 — Repaso: cierre del inverso modular

**Fecha:** 2026-07-09
**Tipo:** Repaso corto (no cuenta como sesión formal nueva de temario — `05-primos-mcd-mcm` ya estaba completo). Cierra el hilo pendiente registrado arriba.

## Objetivo de esta sesión
Cerrar, con el algoritmo de Euclides ya sólido (de `05-primos-mcd-mcm`), el caso de combinación de congruencias con coeficiente≠±1 que había quedado sin resolver formalmente (resuelto por tanteo en la sesión 2 de este tema).

## Recuperación activa inicial
El usuario recordó correctamente el algoritmo de Euclides básico (de la sesión anterior de `05-primos-mcd-mcm`) al calcularlo de nuevo para 9 y 5 sin apoyo. El hueco no estaba ahí, sino en la parte nueva: la sustitución hacia atrás (algoritmo de Euclides extendido) para construir el inverso modular, que nunca se había cubierto.

## Resumen / apuntes

**Por qué hace falta un inverso modular:** en la ecuación 5k ≡ 4 (mod 9), no se puede "dividir entre 5" directamente (no hay fracciones en el mundo de los residuos). Se necesita un entero n que multiplicado por 5 se comporte como "1" en este sistema (5×n ≡ 1 mod 9) — el equivalente entero de 1/5.

**Método del inverso modular (algoritmo de Euclides extendido), en 4 pasos:**
1. Dividir el módulo entre el coeficiente, forma D=d×q+r.
2. Despejar el residuo. Si en un solo paso el residuo ya es 1, seguir al paso 3. Si no, repetir el proceso con la división anterior y sustituir hacia atrás (reemplazar el residuo de un paso dentro de la ecuación del paso anterior) hasta dejar el 1 escrito solo en términos de los dos números originales.
3. Leer el coeficiente que queda multiplicando al número original — ese es el inverso (puede salir negativo).
4. Si salió negativo, sumarle el módulo para pasarlo a un valor positivo equivalente.

**Aplicación a un problema de combinación de congruencias:** una vez que se tiene el inverso n de a (mod b), se nombra la incógnita del primer múltiplo (N=ak), se multiplican ambos lados de "ak = b()+r" por n, y se usa que (a×n) se comporta como 1 (mod b) para dejar k despejado en la forma k=b()+r'. Ver detalle completo con fórmulas en `glosario.json` ("Inverso modular (algoritmo de Euclides extendido)" y "Combinación de dos congruencias por sustitución").

**Nota pedagógica:** el primer intento de explicación (simbólico, con la letra "x" usada a la vez como "por" y como incógnita) no funcionó — ver `dif-04-03` para la estrategia que sí funcionó: ejemplos numéricos chicos progresivos, notación de múltiplos ya conocida en vez de ≡/mod sin anclar, y el usuario ejecutando la mecánica él mismo en vez de seguir una explicación pasiva.

## Ejercicios trabajados
> No se resume ni parafrasea la respuesta del usuario. Cita textual + anotación del agente. Estos ejercicios se resolvieron de forma muy guiada, paso a paso — se cita cada aporte textual del usuario en el orden en que ocurrió.

1. **Enunciado:** Halla el inverso de 3 módulo 7 (n tal que 3×n = 7()+1). Un solo paso de sustitución.
   **Respuesta del usuario (textual, guiada paso a paso):** "7 = 3(2) + 1" → "1 = 7 - 3(2)" → "-2 + 7 = 5"
   **Anotación del agente:** Correcto (verificado: 3×5=15, 15÷7 residuo 1). Primer ejercicio de la serie, con guía en cada paso individual — sirvió para separar la mecánica del método en piezas manejables después de que la explicación simbólica inicial no funcionara.

2. **Enunciado:** Halla el inverso de 4 módulo 9. Un solo paso de sustitución, mismo patrón que el ejercicio 1.
   **Respuesta del usuario (textual):** "4n = 9() + 1 (estoy mejorando en entender la terminología también)

ahora hagamos el euclides en forma D = dq + r

9 = 4(2) + 1

1 = 9- 4(2)

-4(2) = 4n

n = -2 -> n = -2 + 9 = 7

n = 7"
   **Anotación del agente:** Correcto (verificado: 4×7=28, 28÷9 residuo 1). Ya resuelto con bastante más autonomía que el ejercicio 1, aunque la línea "-4(2) = 4n" quedó notacionalmente un poco enredada (mezcla el paso de lectura del coeficiente con la asignación de n) — el resultado y la lógica de fondo fueron correctos.

3. **Enunciado:** Halla el inverso de 3 módulo 8. Dos pasos de sustitución (misma estructura que el problema original de residuo con 5 y 9).
   **Respuesta del usuario (textual, resuelto de forma independiente):** "3n = 8() + 1

8 = 3(2) + 2 -> 2 = 8 - 3(2)
3 = 2(1) + 1

reemplazo:
3 = (8 - 3(2))1 + 1

1 = 3 - (8 - 3(2)) = 3 - 8 + 3(2)

1 = 3(3) - 8

n = 3"
   **Anotación del agente:** Correcto y completo (verificado: 3×3=9, 9÷8 residuo 1), resuelto de principio a fin sin guía turno-por-turno. Es la primera vez que ejecutó bien los dos pasos de sustitución hacia atrás — el punto exacto que le había fallado en el primer intento del problema original (sustituir en la ecuación correcta, la que ya tenía el 1 despejado, no en la que tiene el 5 despejado).

4. **Enunciado:** El problema original, pendiente desde la sesión 2 de este tema: N es múltiplo de 5, y al dividirlo entre 9 deja residuo 4. Si 50<N<95, halla N. Esta vez de principio a fin, sin guía paso a paso.
   **Respuesta del usuario (textual, primer tramo — hallar el inverso):** "haber entonces dices que halle le inverso de 5 en módulo 9, osea 5n = 9() + 1

euclides:
9 = 5(1) + 4  -> 4 = 9 - 5(1)
5 = 4(1) + 1

reem.

5 = 9 - 5(1) + 1
1 = 5 - (9 - 5(1) = 5(2) - 9

entonces:

n =2

ahora no estoy muy seguro de que hacer, si recolecto la información inicial:

N = 5()
N = 9() + 4

entonces:

5() = 9() + 4

y no estoy seguro de que más hacer"
   **Anotación del agente:** El inverso (n=2) salió correcto, incluyendo una autocorrección en el camino (la primera sustitución, "5 = 9-5(1)+1", llevaba a una identidad vacía tipo 5=5 — el mismo tropiezo del primer intento de este problema hace unos días — pero el usuario retomó solo con la sustitución correcta y llegó bien a n=2). Se trabó después al no haberle puesto nombre a la incógnita del primer múltiplo (dejó "N=5()" sin variable), lo que le impedía multiplicar la ecuación por el inverso.
   **Respuesta del usuario (textual, tras corrección guiada — nombrar k y multiplicar por el inverso):** "10k = 9(2()) + 8" → "10k + 18() + 8" → "10k = 18() + 8"
   **Anotación del agente:** Correcto (2×(9()+4) = 18()+8), con dos autocorrecciones de tipeo en el camino.
   **Respuesta del usuario (textual, intento de simplificar 10k a k):** "k = 18() + 8"
   **Anotación del agente:** Incorrecto — el término "9k" (de separar 10k=9k+k) desapareció sin pasar al otro lado. Se corrigió con la regla directa de congruencias (10≡1 mod9 implica 10k≡k mod9) en vez de seguir manipulando el paréntesis con una incógnita adentro.
   **Respuesta del usuario (textual, tras la corrección):** "k = 9() + 8"
   **Anotación del agente:** Correcto.
   **Respuesta del usuario (textual, cierre):** "N = 5k = 5(9() + 8) = 45() + 40

y el valor es el que encontramos antes N = 85"
   **Anotación del agente:** Correcto y completo — N=85, coincide exactamente con el valor hallado por tanteo en la sesión 2 de este tema. Cierra el hilo pendiente con el método formal, no solo con el resultado ya conocido.

**Ronda de confirmación adicional** — el usuario pidió más ejercicios porque un solo problema resuelto no le bastaba para sentirlo sólido. Resueltos de forma independiente, sin guía turno-por-turno:

5. **Enunciado:** Halla el inverso de 4 módulo 11.
   **Respuesta del usuario (textual):** "4n = 11() + 1

11 = 4(2) + 3 -> 3 = 11- 4(2)

4 = 3(1) + 1 -> 4 = 11- 4(2) + 1

1 = 4 - (11 - 4(2)) = 4(3) - 11

n = 3"
   **Anotación del agente:** Correcto (verificado: 4×3=12, 12÷11 residuo 1). Igual que antes, la línea "4 = 11-4(2)+1" es una sustitución en la ecuación "vacía" (confirma 4=4, no aporta), pero el usuario la ignoró y siguió con la sustitución útil sin que se le señalara — ya detecta solo cuándo un camino no lleva a nada.

6. **Enunciado:** N es múltiplo de 4, y al dividirlo entre 11 deja residuo 7. Halla el menor N positivo. (reutilizando el inverso del ejercicio 5)
   **Respuesta del usuario (textual):** "N = 4k
N = 11() + 7

4n = 11() + 1

euclides:

11 = 4(2) + 3 -> 3 = 11 - 4(2)
4 = 3(1) + 1 -> 4 = 11 - 4(2) + 1

1 = 4 - (11 - 4(2)) = 4(3) - 11

n = 3

4k = 11() + 7    (x3)
k = 11() + 21 = 11() + 10

N = 4k = 4(11() + 10) = 44() + 40

el N menor positivo es 44(0) + 40

N = 40"
   **Anotación del agente:** Correcto y completo (verificado: 40÷4=10 exacto, 40÷11 residuo 7). Aplicó "multiplicar por el inverso y reducir el coeficiente" de corrido, sin necesitar que se le desglosara el paso intermedio de 12k≡k (mod 11) — señal de mayor fluidez que en los ejercicios anteriores.

7. **Enunciado:** N es múltiplo de 6, y al dividirlo entre 7 deja residuo 5. Si 100<N<150, halla N.
   **Respuesta del usuario (textual):** "N = 6k
N = 7() + 5
100<N<150
Hallar N

6n = 7() + 5

euclides:
7(5) = 6(5) + 5 -> 5 = 35 - 6(5)
6 = 5(1) + 1 -> 6 = 35 - 6(5) + 1

1 = 6 - (35 - 6(5)) = 6(6) - 35

n = 6

entonces:

6k = 7() + 5   (x6)
k = 7() + 30
k = 7() + 2

N = 6k = 6(7() + 2) = 42() + 12

N = {12, 54, 96, 138, 180, ......}

100 < N < 150

N = 138"
   **Anotación del agente:** Resultado final correcto y completo (verificado: 138÷6=23 exacto, 138÷7 residuo 5; secuencia {12,54,96,138,180} bien construida con diferencia constante 42, sin el error de suma que tuvo en un ejercicio anterior de la sesión de `05-primos-mcd-mcm`). Pero el arranque tiene un desvío conceptual real: el primer paso ("7(5)=6(5)+5") mete un factor ×5 que en realidad es el **residuo del problema** (5), no algo que deba entrar al cálculo del inverso — el inverso de 6 módulo 7 solo depende de esos dos números, nunca del residuo que se busque después. Por coincidencia algebraica el resultado siguió siendo consistente y llegó al n correcto (6, mismo que la versión limpia de un solo paso: 7=6(1)+1 → n=6), pero fue un camino más largo y arriesgado de lo necesario. Señalado al usuario explícitamente como regla a recordar: el residuo del problema real no entra en juego hasta el paso final (multiplicar la ecuación original por el inverso ya encontrado), nunca durante el cálculo del inverso en sí.

## Herramientas usadas
- Cuaderno físico — desarrollo de los 7 ejercicios de esta sesión de repaso (4 del cierre original + 3 de la ronda de confirmación).
- Widget de visualización (`mcp__visualize__show_widget`) — usado una vez para mostrar el mapa completo del algoritmo de Euclides extendido en el ejemplo de 5 y 9, sin depender de LaTeX (evitando el problema ya documentado en memoria de que MathJax no renderiza confiablemente en este entorno).

## Dudas / puntos flojos
- `dif-04-03` (resuelta en esta sesión, confirmada con 3 ejercicios adicionales resueltos de forma independiente): la mecánica del algoritmo de Euclides extendido (sustitución hacia atrás) no cuajó con una sola explicación, incluso entendiendo la lógica conceptual por separado. Se resolvió con práctica progresiva (ver estrategia completa en `dificultades.json`).
- Error de notación detectado y corregido por el propio usuario: usar la misma letra para "multiplicar" y para nombrar una incógnita genera confusión real, no solo estética — a evitar en explicaciones futuras del proyecto.
- Ejercicio 7: metió el residuo del problema (un dato que no pertenece al cálculo del inverso) dentro del algoritmo de Euclides — no causó un resultado incorrecto por coincidencia algebraica, pero es un desvío conceptual real (ver anotación del ejercicio 7). No se registra como dificultad nueva en `dificultades.json` (fue un desliz de un solo caso, corregido en el momento, sin afectar el resultado) — pero **el usuario pidió explícitamente marcarlo para repaso espaciado futuro**, por sentirlo lógicamente confuso pese a haberlo resuelto bien. Ver nota en `dificultades.json` (`dif-04-03`).

## Feynman de cierre
**Consulta del agente (textual):** "Explícame el tema completo de esta sesión como si me lo estuvieras enseñando a mí por primera vez... [se pidió tras completar el problema guiado paso a paso, antes de la serie de ejercicios de práctica]"

**Respuesta del usuario (textual):** "No, la explicación fue tan confusa que aún no termino de entender todo el desarrollo y demás, manda unos ejercicios para ver si logro entender mejor, por que solo con la explicación todo quedó bastante mal"

**Anotación del agente:** No se llegó al Feynman en ese momento — decisión correcta del usuario, priorizar práctica sobre forzar una explicación que no estaba asentada. Tras los 4 ejercicios de práctica (ver arriba), el usuario resolvió el problema original completo sin guía turno-por-turno, lo que es una señal más fuerte de dominio real que un Feynman verbal habría dado en ese momento. No se volvió a pedir un Feynman formal después de los ejercicios — queda como oportunidad para una futura sesión de repaso espaciado (`dificultades.json` / `glosario.json` ya reflejan nivel "sólido").

## Próximos pasos
Hilo del inverso modular cerrado — el usuario se sintió sólido tras la ronda de confirmación (3 ejercicios independientes correctos: n=3 para inverso de 4 mod 11, N=40 y N=138 en dos problemas de aplicación completos). No queda pendiente nada de `04-divisibilidad` para avanzar en el temario.

**Marcado explícitamente por el usuario para repaso espaciado futuro** (lo sintió "lógicamente confuso" pese a resolverlo bien, y no quiere que se olvide): revisar en una sesión futura — sin apoyo paso a paso — un problema nuevo de combinación de congruencias con coeficiente≠±1, y opcionalmente el Feynman completo que no se llegó a hacer en esta sesión. Buen candidato para repasar cuando `dificultades.json` indique que pasaron algunos días desde `fecha_ultimo_repaso` de `dif-04-03` (2026-07-09), según el método de Repetición Espaciada del proyecto.

---

# Repaso — 2026-07-22

**Tipo:** repaso (rama C del PASO 5.5, sesión dedicada de repaso mezclado sobre 5 temas de aritmética vencidos). No cuenta como sesión nueva de temario — `sesiones_dedicadas` sin cambio.

## Ejercicio trabajado
> Cita textual, sin resumir ni parafrasear.

1. **Enunciado:** ¿Cuál es el menor número de tres cifras que al dividirse entre 7 deja residuo 3 y al dividirse entre 5 deja residuo 2?
   **Respuesta del usuario (textual, transcripción de voz):** "La división entre cinco me da una idea. Si para que quede residuo dos, el tercer número [...] debe ser o cero o cinco, para que sea exacta, pero como queda dos, debería ser o dos o siete [...] Luego, simplemente haces el cálculo rápido [...] siete por quince, ciento cinco, luego ciento doce, etcétera [...] El primer número que cumple es ciento veintidós, porque menos tres con el tres de resto queda ciento diecinueve, que es múltiplo de siete, y ciento veintidós te deja un residuo de dos cuando lo divides entre cinco."
   **Anotación del agente:** correcto (122) y con método válido (filtrar por última cifra según el residuo de 5, luego tantear múltiplos de 7 cercanos). El usuario reportó haber consultado el criterio de divisibilidad por 7 durante el ejercicio por curiosidad (confundía si era igual al de 11 y 13), pero él mismo identificó — correctamente — que no lo necesitaba para este problema: la palanca real fue el módulo 5, resuelto sin apoyo. Se cuenta como resolución limpia.

## Repaso intercalado (registro cruzado, ver también los otros 4 README de esta misma sesión)
`04-divisibilidad` — **resultado:** bien (consulta tangencial, no usada en la resolución real) — nivel_repaso: 3 → 4, próximo: 2026-08-05.
