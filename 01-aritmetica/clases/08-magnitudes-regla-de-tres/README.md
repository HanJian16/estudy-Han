# Clase: Magnitudes proporcionales y regla de tres

**Tema en temario:** `08-magnitudes-regla-de-tres`
**Fecha:** 2026-07-11
**Número de sesión para este tema:** 1 (coincide con `sesiones_dedicadas` en `progreso.json`)

## Objetivo de esta sesión
Cobertura completa del tema en una sola sesión: identificar magnitudes DP e IP, resolver reparto proporcional compuesto (2 y 3 magnitudes), y resolver regla de tres simple y compuesta (incluyendo trabajo parcial, rendimiento variable y proporcionalidad con potencias).

## Recuperación activa inicial
Se verificó el prerrequisito directo (razones y proporciones, visto el día anterior, 2026-07-10) con una cuarta proporcional: 3/4 = 9/x. El usuario identificó correctamente extremos (3 y x) y medios (4 y 9), planteó x = (9×4)/3 y llegó a x = 12 sin apoyo. Confirmado como sólido, sin huecos detectados en este prerrequisito.

## Resumen / apuntes

### Magnitudes DP e IP
Dos magnitudes son **directamente proporcionales (DP)** cuando, al aumentar una, la otra aumenta en la misma proporción (y al disminuir una, la otra disminuye). Relación: A/B = k (constante). Ejemplo: cantidad de naranjas y dinero pagado — el precio por naranja es constante.

Dos magnitudes son **inversamente proporcionales (IP)** cuando, al aumentar una, la otra disminuye. Relación: A×B = k (constante). Ejemplo: obreros y días para una obra fija — a más obreros, menos días, y el producto obreros×días (el "trabajo total") se mantiene constante.

No toda pareja de magnitudes es DP o IP: se vio el ejemplo de la edad de una persona y su estatura, que no tienen una relación proporcional definida.

### Reparto proporcional compuesto
Extiende el reparto simple (ya dominado desde 07-razones-proporciones) a más de una magnitud a la vez:
1. Para cada entidad, calcular un **peso**: las magnitudes DP se multiplican, las IP se usan como divisor.
2. Sumar todos los pesos.
3. Calcular k = total a repartir / suma de pesos.
4. Cada entidad recibe k × su peso.
5. Verificar que la suma de las partes dé el total original.

**Ejemplo DP+DP:** empresa reparte 4700 soles entre 3 socios proporcional al capital (2000, 3000, 5000) y al tiempo invertido en meses (3, 2, 4).
Pesos: A = 2000×3 = 6000; B = 3000×2 = 6000; C = 5000×4 = 20000. Suma = 32000.
k = 4700/32000 = 0.146875.
A = 881.25; B = 881.25; C = 2937.5. Suma = 4700 ✓

**Ejemplo DP+IP:** premio de 3900 soles entre 3 estudiantes, DP a horas de estudio (12, 15, 9) e IP a errores (2, 3, 5).
Pesos (horas/errores, porque errores es IP y divide): A = 12/2 = 6; B = 15/3 = 5; C = 9/5 = 1.8. Suma = 12.8.
k = 3900/12.8 = 304.6875.
A = 1828.125; B = 1523.4375; C = 548.4375. Suma = 3900 ✓

**Ejemplo con 3 magnitudes (DP+DP+IP):** bonificación de 7200 soles entre 3 vendedores, DP a años de experiencia (4,6,5) y ventas (30000,20000,40000), IP a quejas (3,2,4).
Pesos: A = (30000×4)/3 = 40000; B = (20000×6)/2 = 60000; C = (40000×5)/4 = 50000. Suma = 150000.
k = 7200/150000 = 0.048.
A = 1920; B = 2880; C = 2400. Suma = 7200 ✓

**Ejemplo con 3 magnitudes (riesgo como divisor):** utilidad de 10800 soles entre 3 socios, DP a capital en miles (15,25,20) y tiempo en meses (8,6,9), IP al riesgo (2,3,4).
Pesos: A = (15×8)/2 = 60; B = (25×6)/3 = 50; C = (20×9)/4 = 45 (en miles). Suma = 155 (miles) = 155000.
k = 10800/155000 = 54/775 ≈ 0.0697.
A ≈ 4180.65; B ≈ 3483.87; C ≈ 3135.48. Suma ≈ 10800 ✓

### Regla de tres simple
Compara una magnitud entre un escenario "antes" y "después".
- Si es **DP**: la razón mantiene el mismo sentido en ambos lados.
- Si es **IP**: la razón de la incógnita se invierte respecto a la conocida.

**Ejemplo IP (obreros-días):** 3 obreros tardan 12 días en una obra → 4 obreros: x = (3×12)/4 = 9 días. Verificación por trabajo total: 3×12 = 36 = 4×9.

**Ejemplo con tasa (grifos):** 1 grifo llena un tanque en 6 horas (tasa = 1/6 por hora). Con 2 grifos idénticos, tasa combinada = 1/6+1/6 = 1/3 por hora → se llena en 3 horas. Es IP (más grifos, menos tiempo).

### Regla de tres compuesta
Cuando intervienen 3 o más magnitudes relacionadas entre sí en un mismo escenario antes/después (no reparto entre entidades distintas).

**Método 1 (razón por razón, invirtiendo según DP/IP):** se arma una razón por cada magnitud (viejo/nuevo). Si la magnitud es DP respecto a la incógnita, la razón va en el mismo sentido; si es IP, invertida. Se multiplican todas.

Ejemplo (ambas IP, obreros-horas-días): 6 obreros × 8 horas × 10 días = 4 obreros × 6 horas × x días. Ambas relaciones son IP respecto a "días": 6×8×10 = 4×6×x → 480 = 24x → x = 20 días.

Ejemplo (una DP, una IP, obreros-metros-días): 5 obreros hacen 40 metros en 8 días. ¿Cuántos obreros para 60 metros en 6 días? Metros es DP respecto a obreros, días es IP respecto a obreros.
5/x = (40/60) × (6/8) → lado derecho = (2/3)×(3/4) = 1/2 → 5/x = 1/2 → x = 10 obreros.

**Método 2 (alternativo, "trabajo por unidad", propuesto por el usuario):** se agrupan en el numerador las magnitudes DP entre sí respecto a una magnitud de referencia (que va en el denominador), y se replica la misma estructura en el otro escenario.

Mismo problema de obreros/metros/días: (5×8)/40 = (x×6)/60 → 1 = 6x/60 → x = 10. Mismo resultado que el método 1 — el usuario mostró que ambos métodos son equivalentes.

**Ejemplo con potencias:** el costo de una plancha metálica circular es DP al cuadrado del radio. Radio 3 cm cuesta 18 soles, ¿cuánto cuesta radio 5 cm?
18/3² = x/5² → 18/9 = 2 → x = 2×25 = 50 soles.

**Ejemplo con trabajo parcial:** 8 obreros hacen una obra en 15 días trabajando 6 horas diarias. Tras 5 días se retiran 3 obreros, y los restantes trabajan 8 horas diarias.
Obra total = 8×6×15 = 720. Obra hecha en 5 días = 8×6×5 = 240. Obra pendiente = 480.
Con 5 obreros y 8 horas: 5×8×x = 480 → x = 12 días. Total = 5+12 = 17 días (2 días más de lo planeado).

**Ejemplo con rendimiento variable (llaves de un tanque):** 5 llaves llenan un tanque en 12 horas. Se dañan 2 llaves (quedan 3) y el rendimiento de las restantes baja a la mitad. Llaves y rendimiento son DP respecto al llenado; horas es IP.
3×1×x = 5×2×12 → 3x = 120 → x = 40 horas.

### Diferencia clave: reparto proporcional vs. regla de tres
El reparto proporcional parte de un total explícito que se divide entre varias entidades: k = total/suma de partes, y cada entidad recibe k × su parte.

La regla de tres no reparte un total entre grupos distintos, sino que compara las mismas magnitudes en dos escenarios (antes y después), usando una razón o producto constante (la k de DP o IP) como ancla. En problemas de "obra total" esa obra total funciona de forma análoga al total del reparto: es la cantidad fija que ancla toda la ecuación, aunque la estructura de resolución sea distinta.

## Ejercicios trabajados
> Regla obligatoria: cita textual, sin resumir ni parafrasear.

1. **Enunciado:** (Recuperación activa) Halla x: 3/4 = 9/x.
   **Respuesta del usuario (textual):** "A ver, si tenemos las razones equivalentes, tres esa cuatro como nueve esa x, tres y x son los extremos, nueve cuatro son los medios. La resultante sería x es igual a nueve por cuatro entre tres. Esa sería la ecuación para hallar x. X sería doce."
   **Anotación del agente:** Identificación correcta de extremos/medios y despeje sin apoyo; confirma prerrequisito sólido.

2. **Enunciado:** Clasifica como DP, IP o ninguna: (a) velocidad de un carro y tiempo que tarda en recorrer una distancia fija; (b) número de páginas de un libro y tiempo de lectura a velocidad constante; (c) edad de una persona y su estatura.
   **Respuesta del usuario (textual):** "A ver, vamos, la velocidad de un carro y el tiempo que tardan en recorrer una estancia fija, ok, la velocidad y el tiempo se multiplican para obtener la distancia. Así que eso es una IP, inversamente proporcional. El número de página es un libro y el tiempo que tardas en leerlo a la misma velocidad de lectura. Es un dp, porque es el número de páginas entre el tiempo que tardas en leerlo, te va a dar una constante, así que es dp. La edad de una persona y su estatura no tiene relación entre sí."
   **Anotación del agente:** Los tres casos correctos, incluyendo el reconocimiento de que no toda relación es DP o IP.

3. **Enunciado:** Una empresa reparte 4700 soles entre 3 socios proporcional al capital (2000, 3000, 5000) y al tiempo invertido en meses (3, 2, 4).
   **Respuesta del usuario (textual, primer intento):** "Vamos a leerlo... lo que haría sería, tengo la tengo el capital, por ejemplo, dos a, ¿no es cierto? Sería el total, que es cuatro mil setecientos entre el capital, que es dos mil y dos mil diez, está multiplicado por la cantidad de meses, tres meses, en este caso... Esto nos quedaría con Seven ceros, cuarenta y siete entre sesenta... y para c sería cuatro mil setecientos entre cinco mil por cuatro... con siete entre doscientos, cuarenta y siete entre doscientos... cero coma setenta y ocho."
   Tras guía correctiva, el usuario retomó y completó correctamente: pesos 6000/6000/20000, k=4700/32000, resultados 881.25/881.25/2937.5, verificados sumando 4700.
   **Anotación del agente:** Reveló confusión inicial entre lógica de regla de tres (división directa) y lógica de reparto (suma de partes primero); ver dif-08-01. Se corrigió con guía y quedó consolidado.

4. **Enunciado:** 3 obreros tardan 12 días en una obra. ¿Cuántos días tardarán 4 obreros?
   **Respuesta del usuario (textual):** "Los traté como directamente proporcionales y son inversamente proporcionales, entonces calculé con una división cuando no debería ser así... x debe ser igual a tres por doce entre cuatro. Cuarta queda tres, esos son nueve. Cuatro obreros hacen el mismo trabajo en nueve días."
   **Anotación del agente:** Autocorrección propia del primer error (había tratado la relación como DP) sin necesidad de guía externa.

5. **Enunciado:** 6 obreros, trabajando 8 horas diarias, hacen una obra en 10 días. ¿Cuántos días tardarán 4 obreros trabajando 6 horas diarias?
   **Respuesta del usuario (textual):** "No hay una relación [entre obreros y horas diarias]... donde si hay una relación es que seis obreros hay en diez días... inversamente proporcional... y trabajando ocho horas diarias, a diez, ahí sí hay una relación... también inversamente profesional. Entonces, quería seis por diez, por ocho... Es igual a las veinticuatro x. X es igual a Tres seis. x es veinte en veinte días."
   **Anotación del agente:** Resuelto de forma limpia e independiente, sin errores ni necesidad de corrección.

6. **Enunciado:** 5 obreros hacen 40 metros de muro en 8 días. ¿Cuántos obreros se necesitan para hacer 60 metros en 6 días?
   **Respuesta del usuario (textual, primer intento):** "Creo que se arman igual. No, no se arman igual, por eso queda cinco por ocho entre cuarenta... Y para el otro lado, que era x días por seis horas al día entre sesenta."
   Tras aclaración conceptual extensa, el usuario propuso y resolvió con su propio método: "agarro un número... x es la cantidad... cinco obreros por ocho días entre cuarenta metros es igual a x por seis días entre sesenta metros" → resultado x = 10 obreros, verificado y explicado con sus propias palabras al final.
   **Anotación del agente:** Ejercicio con mayor necesidad de guía de la sesión (ver dif-08-03); confundió magnitudes con "horas" inexistentes en el enunciado y no distinguió bien cómo invertir la razón IP en el primer intento. Terminó con comprensión profunda al reconstruir su propio método alternativo.

7. **Enunciado:** Un premio de 3900 soles se reparte entre 3 estudiantes proporcional a sus horas de estudio (12, 15, 9) e inversamente proporcional a sus errores (2, 3, 5).
   **Respuesta del usuario (textual):** "Sería dos entre dos, o sea, su peso sería seis" ... "Pues sería seis para cinco para y nueve quintos para hacer... uno coma ocho." Luego, en el cálculo final: "Ok, realicé la suma, me saldría doce coma ocho... treinta y nueve mil entre ciento veintiocho... cuatro mil ochocientos setenta y cinco entre dieciséis... para a, sería... mil ochocientos veintiocho coma ciento veinticinco... para b sería... Mil quinientos veintitrés coma cuarenta y tres... para c sería... Quinientos cuarenta y ocho punto cuarenta y tres... Vamos a comprobar... sale correcto, tres mil novecientos."
   **Anotación del agente:** Ruido de transcripción detectado ("dos entre dos" en la primera frase no cuadra con el peso de 6 citado a continuación; casi seguro dictado de "doce entre dos"), no afecta el razonamiento — los pesos finales (6, 5, 1.8) son correctos. Identificó correctamente que errores (IP) debía ir como divisor del peso, sin que se le diera la respuesta. Ejecución final correcta y autoverificada.

8. **Enunciado:** 1 grifo llena un tanque en 6 horas. ¿En cuánto tiempo lo llenan 2 grifos idénticos?
   **Respuesta del usuario (textual):** "Es directamente proporcional, claro, porque sube la cantidad de grifos. Ah, no, no, no, no, no, eso es inversamente proporcional porque son dos grifos ahora y el tiempo debería ser menos... sería un sexto para uno más un sexto para el doctor, esos son dos sextos que es un tercio por hora. Ahora, en cuanto a su órdenes, se llenará en tres horas."
   **Anotación del agente:** Autocorrección inmediata del primer impulso (DP→IP) y resolución limpia con método de tasas, sin ayuda externa.

9. **Enunciado:** Una bonificación de 7200 soles se reparte entre 3 vendedores proporcional a sus años de experiencia (4,6,5) y ventas (30000,20000,40000), e inversamente proporcional a sus quejas (3,2,4).
   **Respuesta del usuario (textual):** "las que van a ir multiplicando son las inversas, o sea, las quejas y las que van a ir dividiendo son las virtualmente [directamente] proporcionales, que son años y ventas" ... pero en la ejecución: "sería treinta mil por cuatro, y todo esto entre las quejas, que es tres... cuarenta mil... para B... veinte mil por seis entre dos... sesenta mil... para C... cuarenta mil por cinco entre cuatro... cincuenta mil." Resultado final tras autocorrección de un desliz aritmético: "sería mil novecientos veinte más dos mil ochenta, más dos mil cuatrocientos, y ahora sí tenemos los siete mil doscientos, correcto."
   **Anotación del agente:** Lapsus verbal al enunciar la regla (dijo "las DP van a dividir", invertido), pero la ejecución matemática fue correcta (DP multiplica, IP divide) en los tres casos. Autocorrigió un error aritmético a mitad de camino.

10. **Enunciado:** 4 máquinas producen 800 piezas en 5 días. ¿Cuántas piezas producirán 6 máquinas en 8 días?
    **Respuesta del usuario (textual):** "vamos a tomar ochocientos piezas como el valor central... a más máquinas, más piezas, eso es un valor directamente proporcional, se divide ochocientos entre cuatro... a más días, más piezas, eso también es directamente proporcional... sería cuatro por cinco... mi ecuación queda ochocientos entre veinticuatro por cinco es igual a x entre seis... x es igual a cuarenta por cuarenta y ocho es igual a... novecientos veinte."
    **Anotación del agente:** Ruido de transcripción/aritmético detectado: 40×48 = 1920, no "novecientos veinte" (el usuario mismo lo trató como equivalente al resultado correcto en el flujo de la sesión). El planteamiento (800/(4×5) = x/(6×8)) y el método fueron correctos y resueltos de forma independiente. Además articuló espontáneamente la diferencia estructural entre reparto compuesto y regla de tres compuesta.

11. **Enunciado:** El costo de una plancha metálica circular es directamente proporcional al cuadrado de su radio. Si una de radio 3 cm cuesta 18 soles, ¿cuánto cuesta una de radio 5 cm?
    **Respuesta del usuario (textual):** "Como son directamente proporcionales, se van a dividir, entonces, sería algo así como dieciocho entre tres, es igual a x entre cinco... Ahora, es, no, es directamente proporcional a su radio al cuadrado, perdón, entonces, sería tres al cuadrado y cinco al cuadrado, porque sería dieciocho entre nueve es igual a x entre veinticinco. Dieciocho entre nueve es dos, es igual a x entre veinticinco, x es cincuenta, serían cincuenta soles."
    **Anotación del agente:** Autocorrección inmediata (olvidó elevar al cuadrado y se corrigió sin ayuda) y resultado correcto.

12. **Enunciado:** Una utilidad de 10800 soles se reparte entre 3 socios proporcional al capital en miles (15,25,20) y al tiempo en meses (8,6,9), e inversamente proporcional al riesgo (2,3,4).
    **Respuesta del usuario (textual):** "vamos a agarrar directamente proporcionales e inversamente proporcionales... quince mil por ocho entre... eso va a ser sesenta mil para a, para p va a ser veinticinco mil por seis entre tres... cincuenta mil, y para c va a ser veinte mil por nueve entre cuatro... cuarenta y cinco mil... sería diez mil ochocientos en trescientos cincuenta y cinco mil para obtener k... a sería sesenta mil por cincuenta y cuatro entre setecientos setenta y cinco... Cuatro mil ciento ochenta coma sesenta y cuatro... para b... Cuatrocientos ochenta y tres coma ochenta y siete... para c... Tres mil ciento treinta y cinco coma cuarenta y ocho... Corroboramos... salen, sí, aproximadamente los diez mil ochocientos."
    **Anotación del agente:** Ruido de transcripción detectado en dos puntos: "trescientos cincuenta y cinco mil" (debería ser 155000, no 355000, ya que 60000+50000+45000=155000) y "Cuatrocientos ochenta y tres coma ochenta y siete" para B (falta "tres mil" al inicio: el valor correcto es 3483.87, consistente con la verificación final de que la suma da ~10800). El método y la ejecución fueron correctos e independientes.

13. **Enunciado:** 5 llaves llenan un tanque en 12 horas. Se dañan 2 llaves y el rendimiento de las restantes baja a la mitad. ¿En cuántas horas se llena el tanque?
    **Respuesta del usuario (textual):** "Entonces, con esto el planteamiento cambia... Cinco llaves a un rendimiento de de dos, te hace, te llena el tanque en doce horas. Ahora, tres llaves, un rendimiento de uno en x horas... a más llave, menos horas... a más rendimiento, menos horas... x es igual a doce entre cinco por dos... x igual a doce entre diez por tres... x mensuales que serían tres coma seis horas. espérate, bato, me mala mía, mala mía, son inversiones muy proporcionales, no no se dividen, se multiplican. Entonces, x por tres por uno es igual a doce por dos por cinco. Tres x es igual a... x es igual a cuarenta, ahí está. Entonces, seguirá bien cuarenta horas, soy mala mía, tomé y y p, como si fuese división en vez de de que cómo era, de de multiplicación."
    **Anotación del agente:** Primero corrigió un error de comprensión lectora (interpretó "se dañan" como agregar llaves — ver dif-08-04), y luego autocorrigió un segundo error operativo (división en vez de multiplicación — ver dif-08-05) sin ayuda externa, llegando al resultado correcto x=40 horas.

## Herramientas usadas
- Sesión externa (Claude, modalidad: voz con transcripción a texto).

## Dudas / puntos flojos
- No se profundizó en reparto proporcional o regla de tres usando porcentajes directamente (mencionado de pasada como posible variante, no practicado).
- No se trabajó un caso de proporcionalidad inversa combinada con potencias (solo se vio DP con el cuadrado del radio).
- Ver dificultades dif-08-01 a dif-08-05 (todas resueltas en la misma sesión, con guía o autocorrección).

## Feynman de cierre

**Consulta del agente (textual):** "Explícame el tema completo de magnitudes proporcionales y regla de tres, como si me lo estuvieras enseñando a mí por primera vez y yo no supiera nada del tema. Incluye: qué es DP y qué es IP, cómo se resuelve un reparto proporcional compuesto, y cómo se resuelve una regla de tres compuesta (puedes usar cualquiera de los métodos que usamos hoy). Usa tus propias palabras, sin mirar nada de lo que ya escribimos."

**Respuesta del usuario (textual):** "Ok, explicar el tema completo y magnitudes personales y regla de tres. Ok. Magnitudes proporcionales, es cuando tenemos un total que queremos dividir entre la sumatoria de las partes entre las que se quiere repartir, basado en los valores que sostenga. Por ejemplo, si me dicen que vamos a repartir las utilidades de una empresa a cuatro, este, a cuatro trabajadores, ¿ok? Y pero esto va a depender de su cantidad de ventas, de su de sus horas trabajadas y de los errores que hayan cometido, por ejemplo, ¿ok? obviamente, se hace un estudio de cuáles son directamente e inversamente proporcionales, que es directamente proporcional, significa que obtienes más, va directamente relacionado una con otra, o sea, por ejemplo, en este caso, te vas a obtener más más ganancia de las utilidades, si has trabajado más horas, pero es inversamente proporcional, sí. Por ejemplo, funcionan de en sentidos contrarios. Por ejemplo, vas a obtener menos ganancias de la de las utilidades, si es que has cometido más errores, o sea, suben los errores, bajan las las utilidades, directamente proporcional, suben los, este, las horas y suben tus utilidades recibidas. ¿ok? En este caso, es para eso es para lo que funcionan las las variables proporcionales y la regla de tres es una forma de de afianzar entre un antes y un después. Por ejemplo, te pueden decir, ok, dos trabajadores, trabajando cinco horas diarias, te te hacen, no sé, quinientas piezas de de ropa, ¿ok? ¿Cuántos trabajadores harán? No, ¿cuántas piezas harán diez trabajadores a cuatro horas diarias, ¿ok? Entonces, te dan un caso, que se puede equivaler a otro, igualar a otro a a un después, ¿ok? donde tienes las mismas variables y te piden hallar una en específica, una una que no tienes. Haciendo una una ecuación sencilla y verificando cuáles son directa inversamente proporcionales entre sí, puedes generar, este este, la ecuación y despejar el el el problema y hallar cuánto cuántas, en este caso, cuántas piezas se pueden hicieron durante ese periodo y esos trabajadores."

**Anotación del agente:** La explicación mezcló la definición de reparto proporcional (un total dividido entre partes) con la de DP/IP en general, sin distinguir con claridad estructural que el reparto proporcional y la regla de tres son procedimientos distintos (aunque ambos usan la idea de proporcionalidad) — coherente con que esta fue justamente la última duda que el usuario planteó y resolvió después de este Feynman. La intuición de DP/IP en sí (ejemplo de horas trabajadas vs. errores cometidos) fue correcta y clara. El ejemplo de regla de tres que propuso (trabajadores-horas-piezas) fue coherente en estructura aunque no llegó a resolverlo numéricamente dentro de la misma respuesta.

Tras este Feynman, el usuario pidió aclarar dos dudas adicionales antes del cierre: (1) dónde aparece la constante k en la práctica de regla de tres simple DP e IP, y (2) la diferencia entre el "total" del reparto proporcional y el rol análogo del "trabajo total" en regla de tres. Ambas se resolvieron con ejemplos concretos (naranjas/dinero para DP, obreros/días para IP), y el usuario confirmó explícitamente haber entendido la diferencia estructural antes de pedir el handoff.

## Próximos pasos
Comenzar el siguiente tema del curso de Aritmética: **09-porcentajes** (porcentajes y aplicaciones comerciales). El criterio de dominio de 08-magnitudes-regla-de-tres (8/10 sin ayuda) se superó ampliamente con 13 ejercicios trabajados, la mayoría resueltos de forma independiente o con autocorrección propia.
