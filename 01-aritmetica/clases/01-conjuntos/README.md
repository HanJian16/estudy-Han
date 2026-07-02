# Clase: Teoría de Conjuntos

**Tema en temario:** `01-conjuntos`
**Fecha:** 2026-07-02
**Número de sesión para este tema:** 1 (coincide con `sesiones_dedicadas` en `progreso.json`)

## Objetivo de esta sesión
Cubrir el tema completo en una sola sesión: notación básica, cardinalidad con 2 y 3 conjuntos, y leyes de conjuntos (De Morgan) para simplificación.

## Recuperación activa inicial
Antes de explicar nada, se pidió al usuario responder de memoria: (1) qué es un conjunto, (2) qué significan ∈, ⊂, ∅, (3) qué hacen unión/intersección/diferencia, (4) si recordaba el uso de diagramas de Venn.

**Respuesta del usuario (textual):**
> "1. Es una agrupación de elementos.
> 2. No entiendo los simbolos mostrados, me sale algo como $\in$ y no sé que es eso.
> 3. Unión es la agrupación de dos conjuntos, intersección es aquello que comparten dos conjuntos y diferencia es lo que tiene un conjunto (enm este caso A) y que no tiene B (no esta incluido la intersección ya que se comparten entre ambos conjuntos y se quiere todo lo del conjunto A que no tiene B, así que la intersección no va), como dato curioso volvieron a aparecer los simbolos extraños ($A - $B)
> 4. Si los recuerdo, es para ejemplificar de forma gráfica los conjuntos, aquellos elementos que pertenecen al conjunto van dentro del círculo, y su posición varia dependiendo a que conjunto pertenecen, si son de la intersección de conjuntos, o incluso si no pertenecen a ninguno y forman parte de un conjunto universal o mayor (Si tenemos conjuntos sobre conjuntos)"

**Anotación del agente:** Respuestas 1, 3 y 4 sólidas y razonadas (no memorizadas) — en la 3 justificó por qué la diferencia excluye la intersección, en vez de solo repetir una regla. La respuesta 2 no reflejaba un vacío real: fue un fallo de renderizado LaTeX de mi lado (le llegó el código crudo `$\in$` en vez del símbolo). Confirma la hipótesis del perfil: la base de conjuntos tenía más memoria latente de la que el usuario creía; el único hueco genuino era notación pura, nunca vista formalmente en su educación (De Morgan/notación de conjuntos rara vez se enseña explícito en secundaria peruana).

## Resumen / apuntes
- **Notación:** ∈ (pertenece, elemento-conjunto), ⊂ (subconjunto, conjunto-conjunto), ∅ (vacío), n(A) (cardinal).
- **Cardinalidad 2 conjuntos:** n(A∪B) = n(A) + n(B) − n(A∩B).
- **Cardinalidad 3 conjuntos:** n(A∪B∪C) = n(A)+n(B)+n(C) − n(A∩B) − n(A∩C) − n(B∩C) + n(A∩B∩C). Técnica clave enseñada: llenar el diagrama de Venn desde el centro (triple intersección) hacia afuera.
- **Trampa de lenguaje:** "exactamente uno" (solo esa región) vs. "al menos uno" (esa región + todas las que la contienen). Ver dificultad dif-01-01.
- **Leyes de De Morgan:** (A∪B)′ = A′∩B′ y (A∩B)′ = A′∪B′ — complementar invierte tanto los conjuntos como la operación. Doble complemento: (A′)′ = A.
- **Aplicación práctica de De Morgan:** útil cuando los datos de un problema vienen en términos de "no tiene X". El usuario prefiere convertir todo a valores positivos por resta simple (total − "no tiene") y aplicar la fórmula normal, en vez de trabajar directo con conjuntos "primados" — método igual de válido y más seguro para él bajo presión de tiempo.

## Ejercicios trabajados

1. **Enunciado:** Si A = {2, 4, 6} y B = {2, 4, 6, 8}, ¿A ⊂ B es verdadero o falso? ¿Por qué ese símbolo y no ∈?
   **Respuesta del usuario (textual):** "Si A es subconjuinto de B, por que todos los elementos del conjunto A también pertenecen al del conjunto B, así que es verdadero, y es ese simbolo por que el significado es "es subconjunto de", a que estamos comparando dos conjuntos (agrupaciones) y no un elemento y un conjunto como en pertenencia."
   **Anotación del agente:** Correcto y bien justificado. Articuló solo la distinción exacta que suele confundirse en el examen (∈ elemento-conjunto vs. ⊂ conjunto-conjunto). Notación cerrada sin necesidad de más práctica.

2. **Enunciado:** En una encuesta a 80 personas sobre bebidas: 45 toman café, 38 toman té, y 15 no toman ninguna de las dos. ¿Cuántas personas toman ambas bebidas?
   **Respuesta del usuario (textual):** "primero reste 80 (cantidad total), menos 15 (cantidad que no toma ninguna de las bebidas), que me da el n(AuB), eso salió 65 y lo iguale a 45(café) + 38(té) - X, dandome por respuesta 18 que toman ambas bebidas."
   **Anotación del agente:** Correcto (18) y con el método ideal: despejar desde la fórmula en vez de memorizar un resultado. Sin errores ni dudas — cardinalidad de 2 conjuntos dominada al primer intento.

3. **Enunciado:** En un club de 60 personas: 25 practican fútbol, 26 vóley, 26 básquet. 9 fútbol y vóley, 11 fútbol y básquet, 8 vóley y básquet, 4 los tres deportes. ¿Cuántas personas practican exactamente un deporte?
   **Respuesta del usuario (textual, primer intento):** "Los valores son los siguientes, 9 solo futbol, 13 solo voley, 11 solo basquet, 7 solo futbol y basquet, 5 solo futbol y voley, 4 solo voley y basquet, y como ya se sabe 4 todos los deportes, dando un total de 53 personas que hacen los deportes y 7 no hacen ninguno"
   **Respuesta del usuario (textual, tras repregunta del agente sobre qué pregunta responde el 53):** "la pregunta que responde 53 es cuantos practican al menos un deporte, respecto a tu consulta de cuantos practican solo un deporte sería 33"
   **Anotación del agente:** Las 7 regiones del diagrama calculadas perfectamente desde el primer intento (técnica de llenar desde el centro totalmente dominada) — el único error fue de interpretación del enunciado, no de cálculo: confundió "exactamente uno" (33) con "al menos uno" (53). Se autocorrigió de inmediato ante una sola repregunta, sin que se le diera la respuesta. Registrado como dif-01-01 para repaso espaciado por ser una trampa frecuente en el examen UNI, aunque quedó resuelta en la misma sesión.

4. **Enunciado:** Simplificar (A′∩B′)′ usando leyes de De Morgan.
   **Respuesta del usuario (textual, primer intento):** "Se parece al lado derecho de las leyes de arriba, no estoy muy seguro de haber captado, pero para mi el complemento es algo así como "el inverso de", por ejemplo el complemento de A es la inversa de A(Todo lo que no es A), entonces el complemento de una unión es su inverso una intersección, y debe ser la intersección de los inversos de los conjuntos (A -> A') o viceversa, como si fuese una distributiva (x(a+b) = xa+xb), no estoy seguro si hay que mejorar esa comprensión, o quizas tengas otra forma de explicarlo mejor?"
   **Respuesta del usuario (textual, tras guía socrática sobre doble complemento):** "Pues te regresa a lo mismo (A'' = A), y es lo mismo para una unión o intersección si tenemos (A'nB') = ((AuB)')' = (AuB)"
   **Anotación del agente:** Identificó correctamente el patrón (lado derecho de la ley) desde el primer intento; su analogía con la distributiva era una intuición razonable pero imprecisa (De Morgan no solo "reparte", también invierte ∪↔∩ — se le aclaró ese matiz). Con una sola pregunta guía llegó solo a la resolución completa y correcta. Concepto consolidado.

5. **Enunciado:** De 200 postulantes: 130 no postulan a Mecánica, 150 no postulan a Sistemas, y 90 no postulan a ninguna de las dos carreras. ¿Cuántos postulan a ambas?
   **Respuesta del usuario (textual):** "Pues no estoy seguro de haber entendido lo de no hallar quienes si postulan a cada una por separado, lo que hice fue que si me dicen que 90 no postularon, entonces 110 sí, si me dicen que 130 no fueron a mecánica, entonces 70 si (200-130) y así fui ibteniendo n(M), n(S) y n(MuS), con esos 3 volví a armar la formula n(MuS) = n(M) + n(S) - n(MnS) y fue una simple ecuación, pero realmente no entendí el concepto creo, o quizas si lo entendí, pero el lenguaje tecnico me generó dificultades, para mi fue más sencillo pensarlo de forma lógica."
   **Anotación del agente:** Respuesta correcta (10) por una vía distinta a la sugerida: en vez de trabajar con los conjuntos "primados" directamente, convirtió cada complemento a positivo por resta simple y aplicó la fórmula estándar. Es la misma idea de fondo (De Morgan garantiza que ambos caminos dan igual), solo que el usuario evita la notación formal. Se le confirmó explícitamente que sí entendió el concepto — la fricción fue con el lenguaje técnico, no con la lógica — y que su método es incluso más seguro bajo presión de examen. Útil como preferencia de estilo a tener en cuenta en futuras sesiones (ver `dificultades.json`/notas de estilo si se repite el patrón).

## Herramientas usadas
- Cuaderno físico — desarrollo de los cálculos de cardinalidad, como en el examen (sin calculadora).

## Dudas / puntos flojos
- Distinción "exactamente uno" vs. "al menos uno" en problemas de cardinalidad — resuelta en la sesión, pero registrada en `dificultades.json` (dif-01-01) para repaso espaciado por ser una trampa frecuente en el examen UNI.
- Ninguna otra duda pendiente del tema.

## Feynman de cierre

**Consulta del agente (textual):** "Perfecto. Técnica Feynman: explícame Teoría de Conjuntos completo (conceptos, notación, cardinalidad y leyes) como si yo no supiera absolutamente nada del tema — en tus propias palabras, sin usar los términos técnicos como muleta si no puedes respaldarlos con una explicación simple. Tómate el tiempo que necesites; de tu explicación voy a detectar qué quedó realmente sólido y qué necesita un repaso antes de la próxima clase."

**Respuesta del usuario (textual):**
> "Bien, en concepto conjuntos es bastante sencillo, simplemente es una agrupación de varios elementos, las notaciones son sencillas unión para la "unión" de los elementos de dos conjuntos, intersección para ver los elementos que comparten dos conjuntos, pertenencia para poder decir cuando un elemento pertenece (o no) a un conjunto, contiene para saber cuando todos los elementod e un conjunto también pertenecen a otro (tambien llamado subconjunto) o valores como el nulo, cuando un conjunto queda vacío o se presentan aguraciones vacías (como cuando no hay intersección entre grupos), respecto a la cardinalidad es una forma de ir reconstruyendo los valores y a que zonas de un conjunto o diagrama pertenecen, existe la formula para dos o tres conjuntos, pero si vamos a la forma lógica es sencillo de explicar, es sumar los valores de los conjuntos y restar los valores que es están repitiendo, por ejemplo si tenemos a un conjunto A y B y sumamos todos los elementos del conjunto A y todos los elementos del conjunto B notaremos que los elementos que ambos comparten han sido contadas dos veces, así que se debe restar para asegurar que no haya un conteo repetido, eso nos da una formula matemática:
> n(AUB) = n(A) + n(B) - n(AnB), es más sencillo aprender la formula que hacer el calculo lógico ya que a mas conjuntos mas repetición hay, por ejemplo para 3 conjuntos es:
> n(AuBuC) = n(A) + n(B) + n(C) - n(AnB) - n(AnC) - n(BnC) + n(AnBnC)
>
> La formula se pone mas compleja ya que al sumar todos los elementos de A, B y C hemos contados dos veces las intersecciones entre dos grupos y 3 veces la interseccion de los 3 grupos, pero al restar las intersecciones dejamos correcto el conteo para ellas y los grupos sin embargo el conteo de la interseccion de los 3 grupos queda en cero, por lo que se debe volver a sumar dicha intersección.
>
> Desconozco la formula para 4 grupos, pero creo que podría inferirla con el tiempo, voy a intentarlo ahora.
>
> n(AuBuCuD) = n(A) + n(B) + n(C) + n(D) - n(AnB) - n(AnC) - n(AnD) - n(BnC) - n(BnD) - n(CnD) + n(AnBnC) + n(BnCnD) - n(AnBnCnD)
>
> Creo que sería algo así, lo cual es curioso por que no recuerdo haber visto un diagrama de venn de 4 conjuntos (me di cuenta que no sabía dibujarlo al intentar hacerlo en un cuaderno.)
>
> Respecto a las ley de Morgan, es sencillamente trabajar con inversos, por ejemplo tienes al conjunto A y su inversa todo lo que no es A (A') y pasa lo mismo con las uniones o intersecciones siendo inversas, por lo que si tenemos (AuB) su inversa es la inversa de los conjuntos y la inversa de la operación osea (A'nB'), eso nos ayuda a simplificar con la ley de morgan"

**Anotación del agente:** Explicación sin lagunas relevantes en el alcance del tema. Dominó el "por qué" de la fórmula de inclusión-exclusión (no la memorizó, explicó la lógica de doble/triple conteo) y describió De Morgan correctamente como "invertir conjuntos + invertir operación". Como ejercicio extra no pedido, intentó inferir la fórmula de inclusión-exclusión para 4 conjuntos: el patrón general (signos alternados, sumar/restar por niveles de intersección) lo captó bien, pero cometió un error de enumeración combinatoria — con 4 conjuntos hay 4 intersecciones triples posibles (ABC, ABD, ACD, BCD) y solo escribió 2 (ABC y BCD), le faltaron ABD y ACD. No es un vacío del tema en sí (4 conjuntos no se exige en el examen UNI ni tiene diagrama de Venn estándar con círculos, correctamente notado por el usuario), sino una semilla útil para cuando se llegue a Análisis Combinatorio: mostrar cómo enumerar sistemáticamente todas las combinaciones de un tamaño dado en vez de listarlas "a ojo".

## Próximos pasos
Siguiente tema: `02-numeracion` (Numeración y cambio de base). Repasar brevemente al inicio de esa sesión la dificultad dif-01-01 si se presenta la oportunidad (probablemente no antes de 1-3 días, según repetición espaciada).
