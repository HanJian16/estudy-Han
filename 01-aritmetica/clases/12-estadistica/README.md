# Clase: Estadística descriptiva y análisis de gráficos

**Tema principal:** `12-estadistica`
**Tipo de clase:** tema-nuevo — decidido por el PASO 5.5 (rama F: sin continuación, sin repaso vencido, sin cadencia pendiente; siguiente tema no_iniciado del tronco de aprender(obj-2) cuyo prerrequisito ya estaba listo)
**Fecha:** 2026-07-22T19:20
**Número de sesión para este tema:** 1
**Nivel al cerrar:** `nivel_alcanzado` null → **base** (verificado hoy; es el único nivel que obj-2 pide para este tema por ahora)

## Objetivo de esta sesión
Los tres objetivos_aprendizaje completos del tema: calcular media/mediana/moda desde tablas de frecuencias (simples y agrupadas), interpretar gráficos estadísticos extrayendo porcentajes o totales, y resolver problemas que combinan frecuencias relativas con porcentajes.

## Recuperación activa inicial
No aplica — los dos temas previos (09-porcentajes y 10-promedios-mezclas) se estudiaron hoy mismo, así que preguntar qué recordaba de ellos no habría medido retención real (Método 1). Se hizo un puente directo en su lugar: la media ponderada de este tema es la misma fórmula que el promedio ponderado ya dominado, y la frecuencia relativa es el mismo cálculo que el operador porcentual.

## Repaso intercalado
No hubo ningún tema con `fecha_proximo_repaso <= hoy` (el primero vence mañana, 2026-07-23), así que no fue el escape hatch de la rama C. El intercalado vino del piso del 30% del Método 3 (que no depende de haber algo vencido): se tejieron ejercicios de 09-porcentajes (lectura de gráfico circular) y 10-promedios-mezclas (fórmula de promedio ponderado) dentro de la explicación y los ejercicios del tema nuevo, sin sesión de repaso dedicada aparte.

## Resumen / apuntes
- **Tabla de frecuencias:** fi (frecuencia absoluta), hi = fi/total (relativa, es un porcentaje), Fi (acumulada — incluye la propia fila, no solo las anteriores).
- **Media, moda, mediana (datos simples):** media = Σ(xi·fi)/Σfi (mismo promedio ponderado ya conocido); moda = valor con mayor fi; mediana = valor en la posición central, localizada con Fi.
- **Datos agrupados en intervalos:** se usa la **marca de clase** (punto medio del intervalo) en vez del valor suelto, y SIGUE ponderándose por fi — no se promedian las marcas de clase sin peso.
- **Clase modal / clase mediana:** en agrupados, moda y mediana ya no son un valor puntual sino el intervalo completo (el de mayor fi, y el primero cuya Fi alcanza total/2, respectivamente).
- **Lectura de gráficos con porcentajes:** el operador porcentual (×0.35, etc.) ya dominado de 09-porcentajes se aplica igual para sacar cantidades reales desde un gráfico circular/de barras.

## Ejercicios trabajados
> Cita textual, no resumen — regla del proyecto.

1. **Enunciado:** Con la tabla de frecuencias xi=12,14,16,18 y fi=5,10,15,10 (total 40 alumnos), calcular la frecuencia relativa del valor 16 y su porcentaje.
   **Respuesta del usuario (textual):** "Eso sería quince entre cuarenta, que es... cero coma tres siete cinco, que sería treinta y siete coma cinco por ciento."
   **Anotación del agente:** Correcto a la primera. Conectó de inmediato frecuencia relativa con el cálculo de porcentaje que ya dominaba.

2. **Enunciado:** Con la misma tabla, calcular la media aritmética usando xi y fi.
   **Respuesta del usuario (textual):** "El peso cuenta la frecuencia absoluta, que es la cantidad de veces que aparece el valor, entonces sería doce por cinco. sesenta más el catorce por diez, que es ciento cuarenta, más el dieciséis por quince. que es doscientos cuarenta, más dieciocho por diez, que es ciento ochenta. Nos da una suma total de... seiscientos veinte, y eso será entre cuarenta, que es la cantidad total. Quince coma cinco."
   **Anotación del agente:** Correcto (620/40=15.5) y, más importante, dedujo la fórmula él mismo a partir del promedio ponderado ya conocido, sin que se le diera la fórmula de antemano — transferencia limpia.

3. **Enunciado:** ¿Qué porcentaje de los datos tiene un valor de 16 o menos, usando la columna Fi?
   **Respuesta del usuario (textual):** "Pues sería el fi del valor anterior a que se dice seis, o sea, catorce, ¿ok? es quince. Como es si ya tiene el acumulado tanto de catorce como de doce, pues cincuenta gramos quince. Y nos piden el porcentaje, así que quince entre cuarenta. es cero coma tres siete cinco, así que sería treinta y siete coma cinco por ciento."
   **Anotación del agente:** Error de límite: usó el Fi de la fila anterior (14) en vez del Fi de la propia fila pedida (16), que ya incluye a los que valen exactamente 16. Mismo patrón general que sus dificultades históricas de "exactamente vs. hasta/al menos" (dif-01-01, dif-02-05). Se le preguntó qué representa el Fi de la fila de 16 antes de corregir directamente.

4. **Enunciado:** (repregunta sobre el ejercicio 3, tras la pista) — mismo enunciado.
   **Respuesta del usuario (textual):** "Sí, sí, sí, sí, tienes razón. Serán dieciséis o menos. Entonces, claro, tiene que ser con el FBI de la de la fila de dieciséis, que es treinta. Entonces, hace el cálculo. Sería treinta entre cuatro es cero coma setenta y cinco, o sea, setenta y cinco por ciento. Al setenta y cinco por ciento de los datos tiene un valor de dieciséis o menos."
   **Anotación del agente:** Se autocorrigió con una sola pregunta guía, sin que se le diera la respuesta. ("FBI" y "entre cuatro" son ruido de transcripción por voz — el cálculo real fue 30/40=0.75.)

5. **Enunciado:** Con intervalos [10-20⟩,[20-30⟩,[30-40⟩,[40-50⟩ y fi=8,12,15,5, calcular la media agrupada.
   **Respuesta del usuario (textual):** "Pues, para el valor de la media, agarraría la media de cada intervalo y luego haría una media entre cada una de esas medias. Por ejemplo, tenemos diez y veinte, ¿no es cierto? sería treinta entre doce y quince. Entonces, la primera media es quince, la segunda sería veinticinco, la tercera es treinta y cinco, y la cuarta es cuarenta y cinco. Hago la suma de estas, que sería treinta, cuarenta... ciento veinte, ¿ok? Y luego la divido entre cuatro, pues, obviamente. Lo que me deberían treinta, treinta sería la media de este grupo de intervalos."
   **Anotación del agente:** Calculó las marcas de clase correctamente (15,25,35,45) pero las promedió SIN ponderar por fi (dio 30, el promedio simple). Es el mismo patrón de "olvidar aplicar la ponderación al cambiar de formato" ya visto en mezclas — registrado como `dif-12-01`.

6. **Enunciado:** (repregunta, señalando que los pesos de la tabla de frecuencia simple sí se habían usado) — mismo enunciado.
   **Respuesta del usuario (textual):** "tienes razón, no los usé como peso, y eso se me pasó. Entonces, debió haber sido quince por ocho, más veinticinco por doce, más treinta y cinco por quince, más cuarenta y cinco por cinco. Vamos a calcular eso. Quince por ocho es ciento veinte, más veinticinco por doce, que es trescientos, más treinta y cinco por quince, que es quinientos veinticinco, más cuarenta y cinco por cinco, que es doscientos veinticinco. Hagamos la suma. los mil ciento setenta, y se divide entre el total, obviamente, que es cuarenta. Somos veintinueve coma veinticinco. Esa sería la media."
   **Anotación del agente:** Correcto (1170/40=29.25), autocorregido con una sola pregunta guía.

7. **Enunciado:** Con la misma tabla agrupada (fi=8,12,15,5, total 40), ¿en qué intervalo cae la mediana?
   **Respuesta del usuario (textual):** "Pues hay cuarenta valores en total, así que la mediana debería caer entre los valores veinte y veintiuno. A ver, la frecuencia relativa, ok. No, la frecuencia absoluta de estos son ocho, doce, quince y cinco, y su frecuencia relativa... No, la acumulativa y ocho más doce sería veinte. Entonces, el valor veinte debería estar en el intervalo de veinte a treinta."
   **Anotación del agente:** Correcto: [20-30⟩ es la clase mediana (primera con Fi>=20). Identificó bien la posición central pese a un poco de titubeo verbal entre "relativa" y "acumulada".

8. **Enunciado:** Gráfico circular de 320 estudiantes por curso favorito — Matemática 35%, Comunicación 25%, Ciencias 20%, Arte el resto. (1) ¿Cuántos prefieren Matemática? (2) ¿Qué % es Arte? (3) ¿Cuántos prefieren Comunicación o Ciencias?
   **Respuesta del usuario (textual):** "Ahora vamos a analizar el ejercicio, dice, un gráfico circular muestra cómo se distribuyen trescientos veinte estudiantes según su curso favorito. Treinta y cinco por ciento para matemáticas, veinticinco por ciento para comunicación, veinte por ciento para ciencias y arte del resto. Vale, vamos a calcular el arte. Treinta y cinco más veinticinco son sesenta más veinte, ochenta, o sea, arte tiene el veinte por ciento también. Perfecto. ¿Cuántos estudiantes prefieren matemáticas? Ok, tenemos trescientos veinte, ¿ok? Si sabemos que el treinta y cinco por ciento perfil de matemáticas es, podemos multiplicar trescientos veinte por treinta y cinco entre cien, ¿vale? o trescientos veinte por cero coma treinta y cinco, es la forma directa. Es uno dos ciento doce. Entonces, ¿cuántas ventas prefieren matemáticas ciento doce? Que por sesenta g representa arte, ya dijimos el veinte por ciento, por lo primero que calculamos. ¿Cuántas variantes totales prefieren comunicación o ciencias? ¿Comunicación o ciencias cualquiera de los dos significa que son el cuarenta y cinco por ciento,ok? Entonces, cero trescientos veinte por el cuarenta y cinco es cero punto cuarenta y cinco, es ciento cuarenta y cuatro. Entonces, ciento cuarenta y cuatro estudiantes prefieren comunicación o ciencias."
   **Anotación del agente:** Los tres resultados correctos (Arte 20%, Matemática 112, Comunicación+Ciencias 144), resueltos sin ayuda usando el operador porcentual directo. Buen indicio de que ya combina temas sin apoyo (rasgo de nivel intermedio), aunque hoy solo se verificó hasta base porque es lo único que el semestre pide todavía.

## Herramientas usadas
- Cuaderno/cálculo mental — suficiente para todo el tema, sin necesidad de herramienta externa.

## Dudas / puntos flojos
- El olvido de ponderar por frecuencia al pasar a datos agrupados (ejercicio 5) — registrado en `dificultades.json` como `dif-12-01`, resuelto en el momento pero a vigilar si reaparece en un contexto futuro distinto (sería el tercer contexto tras mezclas y esto).

## Feynman de cierre
> Cita textual, no resumen.

**Consulta del agente (textual):** "Explícame estadística descriptiva con tablas de frecuencia como si se lo estuvieras enseñando a alguien que no sabe nada del tema — qué es una tabla de frecuencias, cómo se saca media/moda/mediana, y qué cambia cuando los datos vienen agrupados en intervalos."

**Respuesta del usuario (textual, primer intento):** "Tengo ya una tabla de frecuencias, es una tabla en lo que podemos registrar mediante diferentes columnas el valor de los valores que tenemos. Por ejemplo, las notas que sacaron los alumnos, ¿dale? y la cantidad de veces que se ha repetido estas notas, ¿ok? También podemos poner en otra columna su valor relativo, o sea, el porcentaje, ¿dale? de de estas notas, o cuando el porcentaje total tiene estas notas, y el valor, la frecuencia acumulativa, un valor acumulativo que puede ser otra columna que nos indica, que nos ayuda a saber, este, el total del de la cantidad de valores que que estamos, este, calculando. Por ejemplo, podemos calcular en una en una sala de, en un salón, los la nota de los alumnos y cuántos alumnos están llevando el el curso, por ejemplo, ¿ok? ¿Cómo sacamos la media, moda y mediana? La media es la media aritmética, ¿ok? Podemos sacarla calculando cada valor multiplicado por el peso, o sea, por su frecuencia relativa? No, perdón, por su frecuencia absoluta, ¿ok? Y todo eso dividido entre el total. El total es la sumatoria también de las frecuencias absolutas, o el último valor de la frecuencia acumulada, ¿ok? que que nos da, pues, la la simple y sencilla fórmula de la media, del promedio ponderado, ¿ok? Después eso, la moda es el valor que más se repite, eso es fácil verlo, simplemente viendo el el la columna de la frecuencia absoluta, y y y la mediana es el valor que va justo en el medio. Como al momento de hacer una tabla de frecuencia usualmente ya lo ordenamos, o sea, vamos del valor menor a mayor, va a ser el valor que se encuentra en el medio, o lo podemos sacar fácilmente con la frecuencia acumulada, ¿ok? que nos ayuda a ir contabilizando en qué posición vamos, o con, este, y simplemente tenemos que agarrar el total, dividirlo a la mitad, y ya ya tendríamos la posición en la que debería ir el el valor. Precisa la tabla de frecuencias es es sencillo verlo. de"

**Anotación del agente (primer intento):** Explicación sólida de tablas de frecuencia simple, incluyendo una autocorrección propia (relativa → absoluta como peso de la media). Pero omitió por completo la parte pedida sobre datos agrupados en intervalos — laguna real, no solo un olvido de mencionar. Se hizo una repregunta puntual sobre ese punto específico (Método 5: preguntar sobre el hueco concreto, no repetir la consigna general).

**Respuesta del usuario (textual, tras la repregunta):** "Es cierto, es cierto, cuando vienen agrupados en intervalos, no tenemos como que un valor individual específico. Entonces, ¿cómo hallamos, por ejemplo, la media? Hallamos la media de cada intervalo y luego utilizamos el valor de esta media para como si fuesen los valores normales, ¿cierto? Hallamos los valores de esta de cada de cada una de esas medias y lo contrapesamos con su propio peso en la frecuencia absoluta, y ya podemos sacar la la media, ¿ok? La moda es excelente lo mismo, solo que ya no agarras un valor en específico, sino que agarras el intervalo, el intervalo completo de la moda. Y para la mediana es exactamente lo mismo, también agarras todo el intervalo de que vaya en la posición que se está diciendo."

**Anotación del agente (segundo intento):** Cerró el hueco correctamente: marca de clase como sustituto del valor suelto (sin perder la ponderación esta vez, ya integrada tras el ejercicio 6), y clase modal/mediana como el intervalo completo. Con esto, el tema queda sólido para el nivel base.

## Próximos pasos
Siguiente tema nuevo en la cola de Aritmética: `11-interes-simple` NO aplica (solo sirve a la UNI, que está latente) — el siguiente candidato real depende de qué gane la rama F la próxima sesión entre los cursos base restantes (02-algebra, 03-geometria, 04-trigonometria, 05-fisica) y lo que quede de la cerradura de obj-2. Vigilar `dif-12-01` (olvido de ponderación al cambiar de formato) si reaparece en un futuro tema.
