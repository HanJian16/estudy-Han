# Clase: Razones y proporciones

**Tema en temario:** `07-razones-proporciones`
**Fecha:** 2026-07-10
**Número de sesión para este tema:** 1 (coincide con `sesiones_dedicadas` en `progreso.json`)

## Objetivo de esta sesión
Cubrir el tema completo en una sola sesión (sesión externa, Claude en modalidad texto/voz alternada): plantear razones y proporciones desde enunciados verbales, aplicar la propiedad de series de razones geométricas equivalentes, y resolver problemas con cuarta, tercera y media proporcional.

## Recuperación activa inicial
Al empezar se verificó equivalencia de fracciones (3/4 y 9/12) con producto cruzado, sin errores — sólido, como se esperaba del tema anterior (06-fracciones).

Luego, al plantear si "3 hombres por cada 4 mujeres" es la fracción 3/4, el usuario inicialmente calculó 3/7 y 4/7 (parte sobre el total) en vez de responder directamente sobre 3/4 (parte sobre parte). Al preguntársele por qué hizo ese salto, identificó correctamente la diferencia conceptual él mismo: 3/4 es "la proporción entre hombres y mujeres" (razón) y 3/7 es "el total de hombres que hay en el salón" (fracción parte-todo). No fue un hueco real, sino una comprobación espontánea de que ya distinguía ambos conceptos incluso antes de nombrarlos formalmente.

## Resumen / apuntes

### Razón: definición y tipos
Una razón es una comparación entre dos cantidades, útil cuando no se conoce (o no importa) el total. Hay dos tipos:

- **Razón aritmética (por diferencia):** $a - b = r$. Ejemplo: Juan tiene 20 años, Pedro 12 → razón aritmética $= 20-12=8$ ("Juan tiene 8 años más que Pedro").
- **Razón geométrica (por cociente):** $\dfrac{a}{b} = k$, donde $k$ es el valor de la razón. Ejemplo: mismas edades → $\dfrac{20}{12} = 1.6\overline{6}$ ("Juan tiene aproximadamente 1.67 veces la edad de Pedro"). En $a/b=k$, $a$ es el **antecedente** y $b$ el **consecuente**.

**Diferencia clave con fracción parte-todo:** una razón compara parte contra parte (ej. hombres vs. mujeres, 3/4), mientras que una fracción del tipo visto en 06-fracciones compara parte contra el total (ej. hombres/total = 3/7). Ambas pueden coexistir en el mismo enunciado pero responden preguntas distintas.

### Proporción y propiedad fundamental
Una proporción es la igualdad entre dos razones geométricas con el mismo valor de $k$:

$$\frac{a}{b} = \frac{c}{d} = k$$

Aquí $a$ y $d$ son los **extremos**, $b$ y $c$ son los **medios**.

**Propiedad fundamental de las proporciones:** el producto de extremos es igual al producto de medios:

$$a \times d = b \times c$$

Esto se deriva directamente del producto cruzado de fracciones equivalentes (ya dominado en 06-fracciones) — es el mismo cálculo, con nombre nuevo en este contexto.

### Cuarta, tercera y media proporcional

- **Cuarta proporcional:** dado $\dfrac{a}{b} = \dfrac{c}{x}$, se despeja $x$ mediante ecuación lineal tras cruzar: $a \cdot x = b \cdot c \Rightarrow x = \dfrac{bc}{a}$.
  Ejemplo resuelto: $\dfrac{4}{6} = \dfrac{10}{x} \Rightarrow 4x = 60 \Rightarrow x = 15$. Verificación: $4/6 = 2/3$ y $10/15 = 2/3$ ✓.
- **Tercera proporcional:** caso especial donde el segundo y tercer término coinciden: $\dfrac{a}{b} = \dfrac{b}{x}$.
- **Media proporcional:** $\dfrac{a}{x} = \dfrac{x}{b}$. Aquí $x$ ocupa una posición especial (aparece dos veces: como consecuente de la primera razón y antecedente de la segunda). Al cruzar: $x \times x = a \times b \Rightarrow x^2 = ab \Rightarrow x = \sqrt{ab}$. A diferencia de la cuarta proporcional, no requiere resolver una ecuación lineal sino una raíz cuadrada directa, precisamente porque $x$ aparece en ambos lados de la igualdad cruzada.

### Series de razones geométricas equivalentes
Cuando varias razones tienen el mismo valor de $k$:

$$\frac{a_1}{b_1} = \frac{a_2}{b_2} = \frac{a_3}{b_3} = \cdots = k$$

se cumple que la suma de todos los antecedentes sobre la suma de todos los consecuentes también da $k$:

$$k = \frac{a_1+a_2+a_3+\cdots}{b_1+b_2+b_3+\cdots}$$

Ejemplo verificado: $\dfrac{2}{3} = \dfrac{4}{6} = \dfrac{6}{9} = \dfrac{2}{3}$. Sumando: $\dfrac{2+4+6}{3+6+9} = \dfrac{12}{18} = \dfrac{2}{3}$ ✓.

Esta propiedad es la base del **reparto proporcional**: si se reparte un total en partes proporcionales a números dados $n_1:n_2:n_3$, se asigna $n_1k, n_2k, n_3k$ a cada parte, donde $k$ se halla dividiendo el total entre la suma $n_1+n_2+n_3$.

## Ejercicios trabajados
> Regla obligatoria: cita textual/verbatim de la respuesta del usuario, sin resumir ni parafrasear.

1. **Enunciado:** En una fiesta, la razón entre el número de hombres y mujeres es de 5 a 3. Si llegan 8 hombres más y ninguna mujer más, la nueva razón sería de 3 a 1. ¿Cuántas personas había originalmente en la fiesta?
   **Respuesta del usuario (textual):** "Ok, vamos a resolver lo missing que la proporción de hombres y mujeres en una fiesta, el de cinco a tres. Ok. Y que si llevan ocho hombres más y ninguna mujer más, la nueva razón sería de tres a uno. Vamos a plantear estos. La la proporción original de hombres y mujeres era de cinco k y tres k. ¿Ok? Sea un múltiple de cinco, entre un múltiple de tres. ¿Vale? Y al volver cinco k más ocho entre tres k, la se convierte en una razón nueva de tres a uno. Entonces, cuando tenemos una forma de hacer una modificación cruzada, quede bien allá, allá piden cuántas personas había originalmente en la fiesta antes de que eran los ocho hombres, o sea, me piden cinco k más tres k, que sería ocho k. ¿Ok? Perfecto, vamos a después a a resolver. Por multiplicación cruzada me quedaría que cinco k más ocho es igual a tres por tres k, que es nueve k. Entonces, pasamos todos los k un solo lado, quedaría que mayor cada menos cinco k es igual a ocho, entonces cuatro k es igual a ocho k es igual a dos. Perfecto. Entonces, me vino ya cuántos sabían, habían ocho k. Entonces, habían dieciséis personas originalmente en la fiesta."
   **Anotación del agente:** Planteamiento correcto de variables (5k, 3k), identificación correcta de qué se pide (8k = total original), ecuación cruzada correcta (5k+8=9k), álgebra correcta (4k=8, k=2), resultado correcto (16 personas). Resuelto sin errores, sin ayuda. La transcripción de voz tiene ruido notable ("missing", "modificación cruzada" en vez de "multiplicación cruzada") pero el razonamiento matemático es completamente claro y correcto.

2. **Enunciado:** Se reparte S/. 720 entre tres personas A, B y C en partes proporcionales a los números 2, 3 y 4. ¿Cuánto le corresponde a cada uno?
   **Respuesta del usuario (textual):** "Ok, vamos a resolver. Me dice, se reparte setecientos veinte a veintitrés personas, a, p y c, en partes profesionales a los números dos, tres y cuatro, entonces, como a, como dos k, b como tres k y c como cuatro k. Teniendo en total esos nueve k, nueve k es setecientos veinte, entonces, cada k k es ochenta. Entonces, ¿cuánto le corresponde a cada uno? A le corresponde ciento sesenta, a, b le le corresponde doscientos cuarenta, ya sale corresponde a trescientos veinte. Hacemos la suma para comprobar, cero, cinco, tres, diez, tres, uno a uno, tres, dos, cinco, seis, setecientos veinte, correcto. Entonces, sí, serían las cantidades para cada uno."
   **Anotación del agente:** Aplicación correcta y directa de la propiedad de series de razones equivalentes al reparto proporcional. Planteó 2k, 3k, 4k, sumó a 9k=720, halló k=80, y calculó cada parte (160, 240, 320) correctamente, verificando la suma. Resuelto sin errores, sin ayuda. Transcripción de voz con ruido ("veintitrés personas" en vez de "tres personas", "partes profesionales" en vez de "partes proporcionales") pero sin impacto en el razonamiento matemático mostrado.

## Herramientas usadas
- Sesión externa (Claude, modalidad: texto y voz alternadas / transcripción de voz).

## Dudas / puntos flojos
Ninguna dificultad conceptual real detectada. Las dos correcciones menores durante el Feynman (orden narrativo al nombrar razón aritmética/geométrica, y detalle de por qué la media proporcional se resuelve distinto) fueron de expresión oral, no de comprensión — se aclararon de inmediato al repreguntar puntualmente. No se registran entradas nuevas en `dificultades.json`.

## Feynman de cierre
> Cita textual, no resumen.

**Consulta del agente (textual):** "quiero que me expliques todo el tema de razones y proporciones como si me lo estuvieras enseñando a mí por primera vez, desde cero: qué es una razón, qué es una proporción, la propiedad fundamental, y al menos uno de los conceptos especiales (cuarta, tercera o media proporcional). Usa tus propias palabras, con ejemplos si quieres."

**Respuesta del usuario (textual, primera pasada):** "Ya. Ok, vamos a explicar. ¿Qué es una razón? Una razón es una equivalencia que podemos plantear entre dos partes, sin conocer un todo o sin necesitar conocerlo. ¿Ok? Un una proporción. Por ejemplo, podemos tener un total de de habitantes de una casa. ¿Ok? Podemos decir que la cantidad de habitantes, hombres y la cantidad de habitantes mujeres pueden tener una razón, una proporción de cuatro a tres. ¿Ok? Eso sería como que lo más lo más claro, lo más sencillo. Entonces, eso nos deja una equivalencia así, ¿no? que a entre b nos da una razón, esa es la razón geométrica que nosotros llamamos, porque esa es una razón aritmética que es la resta entre un antecedente y un consecuente. Y la fórmula general nos dejaría algo así, ¿no? como que a es igual a b por k, o b es igual a por k. Bueno, claramente, ya te acabaría dependiendo de de cuál tomamos como antecedente o punto, cómo vamos como consecuente. Y una de las formas más así es de de resolverlo es con la con la aplicación cruzada, que que es lo que tenemos, por ejemplo, tenemos una equivalencia entre entre proporciones a entre b es igual a c entre b y al hacer un el método cruzado de verificar, es para verificar esta equivalencia, no es que a por d es igual a c por b. Entonces, si, por ejemplo, nos dan tres valores, nos falta uno de ellos, podemos calcularlo de esta forma. O usando o, este, Porque nos falta la primera, segunda, tercera o cuarta de las de las razón, de las de los proporciones."

**Correcciones solicitadas y respuesta del usuario (textual, segunda pasada):** "A ver, la razón aritmética es una diferencia entre la el antecedente y el consecuente, mientras que la razón geométrica es una fracción formada por el antecedente y el consecuente. ¿Qué hace especial a la media proporcional? Ah, que nos, que al hacer el el método cruzado queda que x al cuadrado es igual a a por b, entonces, pues ahí ya directamente x como raíz de a por b."

**Anotación del agente:** Primera pasada mostró comprensión conceptual correcta pero con inversión narrativa al nombrar razón aritmética vs. geométrica (dijo "esa es razón geométrica... porque esa es razón aritmética que es la resta", quedando invertido el orden lógico de la frase), y omitió explicar qué hacía especial a la media proporcional pese a mencionarla de pasada. Al repreguntar puntualmente ambos aspectos, el usuario corrigió y explicó ambos con precisión completa en la segunda pasada. El resto de la explicación (definición de razón, ejemplo casa/habitantes, proporción, producto cruzado, aplicación a término faltante) fue clara y correcta desde la primera pasada.

## Próximos pasos
Iniciar el siguiente tema del curso de Aritmética: **08-magnitudes-regla-de-tres** (según `temario.json`, no "porcentajes" como sugirió el resumen externo — el orden real del temario es magnitudes proporcionales y regla de tres antes de porcentajes). Razones y proporciones, su prerrequisito conceptual, quedó con dominio sólido en una sola sesión.
