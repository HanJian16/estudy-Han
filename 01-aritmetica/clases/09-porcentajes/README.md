# Clase: Porcentajes y aplicaciones comerciales

**Tema principal:** `09-porcentajes`
**Tipo de clase:** tema-nuevo — decidido por el PASO 5.5 del `README.md` raíz (siguiente `no_iniciado` en la ruta de aritmética, prerrequisito `08-magnitudes-regla-de-tres` ya en avanzado)
**Fecha:** 2026-07-22T12:20
**Número de sesión para este tema:** 1 (coincide con `sesiones_dedicadas` en `progreso.json`)
**Nivel al cerrar:** `nivel_alcanzado` null → **intermedio** (verificado hoy; avanzado se deja para una sesión futura de repaso camino al examen UNI, decisión explícita del usuario)

## Objetivo de esta sesión
Que el usuario calcule porcentajes directos, entienda por qué los aumentos/descuentos sucesivos se multiplican como operadores (no se suman los porcentajes), y aplique el vocabulario comercial (costo, venta, lista, ganancia/pérdida) en problemas combinados tipo examen.

## Recuperación activa inicial
Bridge desde `07-razones-proporciones` (última sesión real: 2026-07-10, no hoy): se le pidió plantear "qué fracción de 100 representa 1880 de 4700" apoyándose en razón geométrica / cuarta proporcional. El usuario reconoció que su memoria había "vacilado un poco" sobre razones geométricas, pero reconstruyó el planteamiento correcto por su cuenta (método del "valor de 1%": 4700/100=47, luego 1880/47=40 → 40%), y después conectó explícitamente ese resultado con la cuarta proporcional (1880/4700 = x/100) que ya tenía sólida. Fortaleza clara: puede reconstruir un procedimiento antiguo razonando desde cero en vez de solo recordarlo de memoria.

## Repaso intercalado
No hubo repaso formal intercalado (ningún tema de `mantener()` tenía `fecha_proximo_repaso <= hoy`: los más próximos vencían mañana 2026-07-23). Sí hubo conexión activa con `07-razones-proporciones` (cuarta proporcional) como puente conceptual, sin ejercicios de repaso dedicados de ese tema.

## Resumen / apuntes

**Definición y cálculo directo:** x% de A = (x/100) × A. Equivalencias trabajadas: 40% = 40/100 = 2/5 = 0.4.

**Multiplicador porcentual (operador):**
- Aumento de x%: multiplicador = (1 + x/100).
- Descuento de x%: multiplicador = (1 − x/100).
- Se multiplica el total por el multiplicador directamente — no se suma/resta el porcentaje sobre el total en dos pasos separados.

**Porcentajes sucesivos (no se suman los porcentajes, se multiplican los operadores):** un aumento de 20% seguido de un descuento de 20% NO vuelve al valor original — el segundo porcentaje actúa sobre el nuevo valor (ya modificado por el primero), no sobre el original. El operador combinado es el producto de ambos multiplicadores (ej. 1.20 × 0.80 = 0.96 → efecto neto −4%, no 0%).

**Vocabulario comercial:**
- Precio de costo (Pc): lo que cuesta comprar/producir.
- Precio de lista (Pl): precio marcado, ANTES de cualquier descuento.
- Precio de venta (Pv): lo que realmente se cobra, DESPUÉS del descuento sobre el precio de lista.
- Ganancia = Pv − Pc (si es negativo, es pérdida).
- % de ganancia sobre el costo = (Ganancia/Pc) × 100.

**Punto de fricción real de la sesión (no del usuario, del agente):** la primera explicación del "multiplicador" se dio de forma abstracta (fórmula con letras primero) y generó confusión real, aunque el usuario ya sabía aplicar el concepto de fondo. Al reexplicar anclando en el número concreto ya validado (0.40 = 40/100 de antes, y remarcando que la operación es × no ÷), la confusión se resolvió de inmediato. Ver `dificultades.json` — no se registró como dificultad del usuario porque no produjo ninguna respuesta incorrecta; queda documentado como preferencia de enseñanza en la memoria del agente (explicaciones concretas antes que abstractas).

## Ejercicios trabajados

1. **Enunciado:** (bridge desde razones/proporciones) "¿Cómo plantearías, usando razón geométrica, qué fracción de 100 representa 1880 de un total de 4700?"
   **Respuesta del usuario (textual):** "Mi memoria vaciló un poco, porque hace ya varios días que llevé la sesión de razones geométricas antecedentes otros consecuentes, pero vamos a intentar, este, generar una ecuación que nos ayude a liberar esto. A ver, si quisiéramos establecer qué fracción de cien representa una cantidad de respeto como tal, ok, si el total es cuatro mil setecientos, ¿ok? y queremos una fracción de cien, entonces sería cuatro mil setecientos entre cien. Eso nos daría cuarenta y siete, ¿ok? Y esto debe ser multiplicado por un valor que nos dé el total de mil ochocientos ochenta. ¿Ok? Así podríamos hallar como que la parte ese ese esa multiplicación nos daría el el la parte cien, que es mil ochocientos ochenta de cuatro mil setecientos."
   **Anotación del agente:** Planteó correctamente el método del "valor de 1%" (4700/100=47) por su cuenta, sin que se le enseñara antes. Solo le faltó completar el segundo paso (cuántas veces cabe 47 en 1880), que se guio con una pregunta elaborativa en vez de dárselo.

2. **Enunciado:** "¿Qué representa ese 47 en el contexto del problema?"
   **Respuesta del usuario (textual):** "Pues sería el uno por ciento, cuarenta y siete sería el uno por ciento de cuatro mil setecientos."
   **Anotación del agente:** Correcto e inmediato.

3. **Enunciado:** "Si 47 equivale a 1%, ¿cuántas veces cabe ese 47 dentro de 1880? ¿Qué representa ese número en porcentaje?"
   **Respuesta del usuario (textual):** "sacar mil ochocientos ochenta, entre 47, Nos daría cuarenta, entonces, cuarenta y siete y cabe cuarenta veces entre mil ochocientos ochenta, y como cada cuarenta y siete es uno por ciento, eso sería el cuarenta por ciento. El cuarenta por ciento del total, obviamente, que es cuatro mil setecientos. Entonces, mil ochocientos ochenta es cuarenta por ciento de mil, de cuatro mil setecientos."
   **Anotación del agente:** Completó el razonamiento correctamente hasta el resultado final (40%).

4. **Enunciado:** "¿Por qué crees que 1880/4700 = x/100 es la misma ecuación que armaste con el valor de 1%? ¿Qué representa el 100 en esa proporción?"
   **Respuesta del usuario (textual):** "Porque queremos hallar la relación, o sea, la relación de mil ochocientos ochenta entre cuatro mil setecientos es la misma relación que debe haber entre x y cien, así x sería el porcentaje correcto en base a cien, tomando el total de cien como cuatro mil setecientos, y la proporción de x en base a mil ochocientos ochenta."
   **Anotación del agente:** Conectó correctamente el método propio con la cuarta proporcional ya sólida de `07-razones-proporciones`. Evidencia de transferencia conceptual real, no memorización aislada.

5. **Enunciado:** "¿Qué multiplicador usarías para un aumento de 15% sobre 200? ¿Y para un descuento de 15%?"
   **Respuesta del usuario (textual, primer intento, con confusión declarada):** "La verdad, no entendí tu explicación de los multiplicadores, o sea, me sale que aparecería el cuarenta por ciento, cuatro mil setecientos de un solo golpe, entre paréntesis, cuatro mil setecientos barra times, cero punto cuarenta, es igual a mil ochocientos ochenta. No soy seguro, creo que eso sería cuatro mil setecientos entre cero punto cuarenta, ¿verdad? Mismo resultado que ya verificaste, cero punto cuarenta es dos quintos, cuarenta entre cien. La verdad, no entendí a qué rayos te referías, y para el aumento de descuento tampoco. ¿Me dices que el multiplicador es uno más x entre cien y uno menos entre x entre cien, pero, o sea, multiplicador por qué? O sea, ¿qué vas a hacer? ¿Vas a multiplicar el total por eso? O sea, que en este caso sería como doscientos por uno más quince entre cien, así. O sea, que eso quedaría ciento quince entre cien, entonces doscientos por ciento quince entre cien, algo así, no, no entiendo el planteamiento, no lo has explicado correctamente."
   **Anotación del agente:** La confusión fue real pero, revisando su propio texto, el planteamiento del aumento (200 × (1+15/100)) ya estaba bien encaminado — el problema fue que mi primera explicación fue abstracta (fórmula con letras antes que número concreto) y el usuario tuvo que inferir demasiado. Se reexplicó anclando en el número ya validado (0.40 = 40/100) y remarcando el operador (× no ÷). Ver feedback guardado en memoria del agente.
   **Respuesta del usuario (textual, tras la reexplicación):** "Ya entendí. Entonces, este, para un descuento del quince por ciento, sería cero punto ochenta y cinco, doscientos por cero punto ochenta y cinco, que sería, déjame calcularlo. Ciento setenta. Claro, ahora ya entendí porque la la explicación fue más, fue mejor, ¿vale? Porque la anterior explicación no se entendió nada. Tuvo que inferir un montón de cosas y, aunque llegué a la respuesta correcta, pues me costó."
   **Anotación del agente:** Correcto (200×0.85=170) y feedback explícito y valioso sobre el estilo de explicación, ya guardado en memoria persistente del agente.

6. **Enunciado:** "100 sube 20% y luego ese nuevo precio baja 20%. ¿Vuelve a 100? ¿Por qué sí o no?"
   **Respuesta del usuario (textual):** "Ok. Sí, es correcto. El primer paso es cien por uno punto veinte, que es la subida del veinte por ciento. Dale, ciento veinte, y el segundo descuento baja el veinte por ciento. ¿Ok? Pero sobre el total, no sobre el valor original, entonces no va a regresar Así en, sino que va a ser ciento veinte por cero punto ocho, que es noventa y seis. Regresa a noventa y seis."
   **Anotación del agente:** Correcto, con la razón bien identificada (el segundo porcentaje actúa sobre el valor ya modificado).

7. **Enunciado:** "Si multiplicas 1.20 × 0.80 directamente, ¿qué te da? ¿Qué te dice eso sobre hacerlo en un solo paso?"
   **Respuesta del usuario (textual):** "El valor del multiplicador sería cero punto noventa y seis, y sí, daría exactamente el el mismo resultado comparado con el, o sea, si los valáramos si lo valuáramos sobre el el precio inicial y sin tener que hacer el paso medio."
   **Anotación del agente:** Generalizó correctamente la regla de multiplicar operadores para sucesivos, sin que se le diera la regla directamente.

8. **Enunciado:** "Un comerciante compra un artículo en 60 soles y lo vende en 75 soles. ¿Cuál es su ganancia en soles, y qué porcentaje representa sobre el costo?"
   **Respuesta del usuario (textual):** "Su ganancia en soles sería quince, quince soles, y para calcular el porcentaje de ganancia sobre el costo sería quince entre sesenta por cien, quince entre sesenta es... cero punto veinticinco, y eso por cien nos daría el veinticinco por ciento de ganancia sobre el costo."
   **Anotación del agente:** Correcto y directo, sin ayuda.

9. **Enunciado (Problema A):** "A un artículo se le hacen dos descuentos sucesivos de 10% y 20%. ¿A qué descuento único equivalen?"
   **Respuesta del usuario (textual):** "Si se les hace descuentos sucesivos, sería, pues, el primero del diez por ciento sería un cero punto noventa. ¿Ok? Y el segundo del veinte por ciento sería un cero punto ochenta, eso nos da un cero punto setenta y dos. Ese sería el descuento sucesivo serían del veintiocho por ciento en total."
   **Anotación del agente:** Correcto (0.9×0.8=0.72 → 28% de descuento único).

10. **Enunciado (Problema B):** "Un comerciante compra un artículo en 200 soles. Lo marca con un precio de lista tal que, al dar un descuento del 20% sobre ese precio, todavía lo vende en 220 soles. ¿Cuál fue el precio de lista?"
    **Respuesta del usuario (textual):** "A ver, vamos con el problema de un comerciante compra un artículo de doscientos soles. Ok, ese es el precio de compra. Lo marcas con un precio listo tal que al dar el el descuento del veinte por ciento sobre ese precio, todavía lo vende en doscientos veinte soles. Ok. ¿Cuál fue el precio de lista? Ok. Dice que al dar un precio de descuento x, ¿ok? por un descuento por del veinte por ciento sería x por cero punto ochenta es igual a doscientos veinte. Pasamos el cero punto ochenta dividir, entonces aquí nos daría doscientos veinte entre cero punto ocho, eso es doscientos setenta y cinco. Entonces, el precio de lista era doscientos setenta y cinco."
    **Anotación del agente:** Correcto (x×0.8=220 → x=275). Manejo fluido de la ecuación inversa con incógnita en el precio de lista.

11. **Enunciado (Problema C):** "En una encuesta a 800 personas, el 25% prefiere fútbol. De esas personas, el 40% son mujeres. ¿Cuántas mujeres prefieren fútbol?"
    **Respuesta del usuario (textual):** "Vamos con nosotros dos. Problemas porcentaje encadenado sobre una cantidad no un precio. En una encuesta, ochocientas personas. Ok. El veinticinco por ciento prefiere el fútbol. Ok. De esas personas, el cuarenta por ciento son mujeres. ¿Cuántas mujeres prefieran el fútbol? Eso es prácticamente lo mismo que el problema sería. El veinticinco por ciento sería cero punto veinticinco por, ¿ok? Porque sobre s se pone el cuarenta por ciento que son mujeres, sería cero punto cuatro. Entonces, nos da un cero punto uno. o sea, el diez por ciento, y eso de ahí por ochocientos sería ochenta. Entonces, ochenta mujeres prefieren el fútbol."
    **Anotación del agente:** Correcto (800×0.25×0.40=80). Aplicó la misma lógica de operadores encadenados a una cantidad de personas, no solo a precios — buena señal de generalización del concepto.

12. **Enunciado (Problema D):** "Un artículo cuesta S/150, con ganancia del 20% sobre el costo incluida en el precio de lista, y luego un descuento del 10% sobre ese precio marcado. ¿A cuánto se vendió finalmente y el vendedor gana o pierde respecto a su costo?"
    **Respuesta del usuario (textual):** "Segundo, vamos con el problema de, combinando costos, más ganancia, más descuento, típico de examen, costos, ganancia, descuento, ok. Un artículo cuesta ciento cincuenta, ese es el precio de costo, ¿ok? No. Sí, es el precio de costo. Ok. Se marca con un precio de lista que incluye una ganancia del veinte por ciento sobre el precio de costo. Ok. Entonces sería, vamos a calcularlo una ganancia del veinte por ciento, sería uno coma veinte, por sobre el precio del costo, que es ciento cincuenta, nos daría ciento ochenta, el precio de lista, es ciento ochenta. Luego se hace un descuento del diez por ciento sobre ese precio marcado, o sea, que a ciento ochenta sería un descuento del diez por ciento, sería cero punto nueve, ciento sesenta y dos. Ese sería el precio con descuento. ¿A cuánto se venía finalmente y el vendedor sigue ganando o pierde respecto a su costo? Se vendió finalmente a ciento sesenta y dos por el descuento sobre el precio de lista y sigue ganando, gana doce soles."
    **Anotación del agente:** Correcto en cada paso (Pl=150×1.20=180; Pv=180×0.90=162; ganancia=162−150=12, sigue ganando). Es el problema más exigente de la sesión (combina 3 operaciones porcentuales encadenadas con vocabulario comercial) y lo resolvió sin errores ni ayuda — evidencia central de nivel intermedio.

## Herramientas usadas
- Widget MathJax (visualización en vivo) — para la definición de porcentaje, la equivalencia entre el método del "valor de 1%" y la cuarta proporcional, y la corrección de la confusión sobre el multiplicador.

## Dudas / puntos flojos
- **Precio de lista vs. precio de venta:** en el Feynman de cierre, el usuario definió "precio de lista" como "el precio estimado al que se desea vender... el precio original al que debería venderse", sin distinguirlo explícitamente de "precio de venta" (lo que se cobra DESPUÉS del descuento). En la práctica lo aplicó bien en el Problema D (Pl=180 → Pv=162 tras el descuento), así que es una imprecisión verbal, no un error de cálculo. Se corrigió en el momento con una frase ancla: "lista = marcado, antes del descuento; venta = lo que realmente se cobra, después." No se registra en `dificultades.json` por no haber producido ningún error real — vigilar de forma pasiva en la próxima sesión que toque este tema (repaso o la clase de estadística que lo usa como prerrequisito).
- La confusión inicial sobre el "multiplicador" fue de origen del agente (explicación abstracta antes que concreta), no del usuario — documentada como feedback de enseñanza en la memoria persistente del agente, no aquí.

## Feynman de cierre
**Consulta del agente (textual):** "explícame el tema de hoy (porcentajes y sus aplicaciones comerciales) como si se lo estuvieras enseñando a alguien que no sabe nada del tema. Cubre qué es un porcentaje, cómo se calcula directo, qué pasa con aumentos/descuentos (sobre todo los sucesivos), y los términos comerciales (costo, venta, lista, ganancia). Tómate el tiempo que necesites — entre más completo, mejor detecto si algo quedó suelto."

**Respuesta del usuario (textual):** "Lo mando aquí mismo, para no tener que estar mandando dos dos prompts distintos, así que agarro. El tema de hoy, porcentajes de sus aplicaciones comerciales, ¿ok? ¿Qué es un porcentaje? Es una parte, ¿ok? del un total, ¿ok? ¿Cómo se calcula directo fácilmente? Agarras el total y lo multiplicas por la parte del porcentaje que deseas. Por ejemplo, yo quiero saber el cuarenta por ciento de ochocientos. Entonces, hago ochocientos por cero punto cuarenta, que es el equivalente en decimales al cuarenta por ciento, cuarenta sobre cien, y eso me daría el resultado. ¿Ok? Ese es el cálculo directo. ¿Qué pasa con aumento de descuentos, sobre todo los sucesivos? Fácil. Si yo, por ejemplo, tengo doscientos como como el precio de unas zapatillas, y me dicen que hay un aumento del veinte por ciento y luego un aumento del diez por ciento, eso significaría que podíamos hacer una multiplicación sencilla. El aumento del veinte por ciento sería ciento veinte, y el aumento del diez por ciento sería ciento diez, sobre, ambos sobre cien, o sea, uno punto veinte y uno punto diez. Entonces, al multiplicar entre ellos... nos daría uno punto treinta y dos. Es que sería el aumento total. Entonces, para hallar el valor de las zapatillas que o costalmente, originalmente costaban doscientos, sería uno punto treinta y dos por doscientos, te nos daría doscientos sesenta y cuatro. Por ejemplo, eso sería un incremento. Ahora, un descuento sería al revés, ¿no es cierto? Si, por ejemplo, me dicen que tengo un descuento del diez por ciento y luego un descuento del quince por ciento, sería de uno, sería de uno el diez por ciento sería cero punto noventa, y del otro el quince por ciento sería cero punto ochenta y cinco, lo mismo, cero punto noventa por cero punto ochenta y cinco, nos daría un descuento total de cero punto setenta y seis, cinco, ¿ok? y eso lo multiplicamos Por el precio original, que sería doscientos y daría ciento cincuenta y tres. Entonces, esos los descuentos sucesivos darían ciento cincuenta y tres. Perfecto. ¿Terminas comerciales, fáciles, precio de costo, el precio al que te cuesta comprar algo, el precio de venta, el precio al que se quiere vender algo, precio de lista, el precio estimado al que se desea vender, ¿ok? O como que el precio original que debería al que debería venderse. Ganancia, la diferencia entre el precio al que se vendió, ¿ok? y el precio que costó. Si el precio al que se vendió es menor al precio que costó entonces estás en perdida."

**Anotación del agente:** Explicación completa y sólida: cálculo directo con ejemplo propio correcto (800×0.40), lógica de sucesivos con dos ejemplos propios verificados numéricamente (aumento: 1.20×1.10=1.32→264; descuento: 0.90×0.85=0.765→153), y vocabulario comercial cubierto en su mayoría. Único punto flojo: la definición de "precio de lista" quedó difusa (no distinguida explícitamente de "precio de venta"), aunque la aplicó bien en los ejercicios — ver "Dudas / puntos flojos" arriba.

## Próximos pasos
`09-porcentajes` queda `completado` (flujo) en `nivel_alcanzado: intermedio`, con escalera de repaso armada (`nivel_repaso: 1`, próximo repaso 2026-07-23). Avanzado se deja pendiente a propósito para una sesión de repaso más cerca del examen UNI (cuando ese objetivo despierte o lo pida un repaso dedicado) — no se abre una nueva sesión solo para esto. Siguiente tema nuevo en la ruta de aritmética: `10-promedios-mezclas` (prerrequisito: este tema, ya en nivel suficiente).
