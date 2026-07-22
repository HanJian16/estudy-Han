# Clase: Promedios, mezclas y aleaciones

**Tema principal:** `10-promedios-mezclas`
**Tipo de clase:** tema-nuevo — decidido por el PASO 5.5 del `README.md` raíz (rama F; ninguna rama A-E aplicaba, el único repaso vencido —`01-conjuntos`— solo sirve a la UNI, en `latente`)
**Fecha:** 2026-07-22T15:10
**Número de sesión para este tema:** 1 (coincide con `sesiones_dedicadas` en `progreso.json`)
**Nivel al cerrar:** `nivel_alcanzado` = `base` — techo verificado hoy porque es el único nivel que algún objetivo ACTIVO (obj-2, vía cerradura) pide para este tema; la UNI (que pediría `avanzado`) está en `latente` y no cuenta.

## Objetivo de esta sesión
Los tres `objetivos_aprendizaje` completos del tema: (1) calcular y relacionar los tres promedios clásicos (MA ≥ MG ≥ MH), (2) resolver precio medio en mezclas, (3) resolver problemas de aleaciones y ley (quilates).

## Recuperación activa inicial
No aplica — el prerrequisito directo (`09-porcentajes`) se estudió hoy mismo, en la sesión inmediatamente anterior (12:20-13:02). Se arrancó directo conectando con `07-razones-proporciones` (reparto proporcional, no estudiado hoy) como puente hacia el promedio ponderado.

## Repaso intercalado
No hubo repaso formal de temas vencidos (ninguno vencido dentro del universo de mantenimiento de obj-2 hoy). Hubo puentes conceptuales sin ejercicios dedicados aparte:
- `07-razones-proporciones` (reparto proporcional, k = total/suma de pesos) → usado para introducir el promedio ponderado por analogía.
- `09-porcentajes` (multiplicador porcentual, porcentajes sucesivos) → usado para dar intuición del promedio geométrico como "preserva el producto".

## Resumen / apuntes

**Promedio Ponderado (PP)** = (Σ peso·valor) / (Σ peso). El promedio aritmético normal es el caso particular con todos los pesos = 1 (PP ⊃ MA, no al revés).

**Los tres promedios clásicos, por qué cantidad preservan:**
- **MA** (aritmético) = (a₁+...+aₙ)/n — preserva la SUMA.
- **MG** (geométrico) = ⁿ√(a₁·...·aₙ) — preserva el PRODUCTO. Se usa para tasas de crecimiento sucesivas (mismo mecanismo que "porcentajes sucesivos").
- **MH** (armónico) = n/(1/a₁+...+1/aₙ) — preserva el TOTAL de la "otra" cantidad, cuando el numerador de una razón se mantiene FIJO entre los casos (misma distancia, mismo gasto) y lo que varía es la tasa.
- **Propiedad:** MA ≥ MG ≥ MH siempre (igualdad solo si todos los valores son iguales). Verificado con 40 y 60: 50 ≥ 48.99 ≥ 48.

**Precio medio en mezclas** y **ley de aleaciones** son ambos aplicaciones directas de PP: (Σ cantidad·precio)/(Σ cantidad) y (Σ peso·ley)/(Σ peso) respectivamente. El divisor es la SUMA de todas las cantidades, no una sola.

## Ejercicios trabajados

1. **Enunciado:** Promedio ponderado de notas: práctica (peso 1) = 14, examen (peso 2) = 18.
   **Respuesta del usuario (textual):** "Sería la prima nota de la primera práctica por uno, que es catorce, y más la nota de la segunda práctica, que es dieciocho, por dos. que es treinta y seis. Eso nos daría un total de treinta y seis, cuarenta cincuenta [...] Y eso debemos dividirlo sobre la cantidad total de pesos, o sea, entre tres. Que me daría dieciséis coma seis, aproximadamente."
   **Anotación del agente:** Correcto de principio a fin (14×1+18×2=50, 50/3≈16.67), incluyendo el paso que suele fallar (dividir entre la suma de pesos, no entre la cantidad de valores) — lo dedujo solo por analogía con reparto proporcional, sin que se le diera la fórmula antes.

2. **Enunciado:** Velocidad promedio de un viaje ida (40 km/h) y vuelta (60 km/h), misma distancia — ¿es 50 km/h (MA)?
   **Respuesta del usuario (textual, primera intuición):** "Sí, creo que es cincuenta kilómetros por hora [...] es el mismo peso, es un promedio aritmético, ya no es un promedio ponderado."
   **Anotación del agente:** Cayó en la trampa esperada (intuición ingenua de que pesos iguales en distancia = MA). Se le pidió verificar con números concretos en vez de corregirlo directamente.
   **Respuesta del usuario (verificación, textual):** "de ida tardaría tres horas. De regreso, tardaría dos horas [...] ciento veinte entre tres es cuarenta, ciento veinte entre dos es sesenta [...] sería doscientos cuarenta kilómetros [...] entre el tiempo de ida y vuelta, son cinco horas [...] Cuarenta y ocho kilómetros por hora. No es el mismo. Qué raro."
   **Anotación del agente:** Se autocorrigió con el cálculo directo (240/5=48) y notó la discrepancia con su propia intuición sin que se le señalara. A partir de ahí identificó solo que el tiempo (no la distancia) era el peso real, lo que lo llevó a MH.

3. **Enunciado:** Verificación de MH con fórmula: MH(40,60).
   **Respuesta del usuario (textual):** "sería doscientos cuarenta entre cinco [...] Cuarenta y ocho" — y al aplicar la fórmula 2/(1/40+1/60) llegó al mismo 48, confirmando que la fórmula reproduce el cálculo manual.
   **Anotación del agente:** Correcto.

4. **Enunciado:** Tasa de crecimiento anual promedio equivalente a 10% el primer año y 20% el segundo.
   **Respuesta del usuario (textual):** "El operador del baño uno es uno diez [...] del año dos es uno veinte [...] uno treinta y dos [...] pero no es la tasa de crecimiento promedio anual [...] sacarle raíz [...] uno punto catorce ochenta y nueve [...] Representa cerca del catorce por ciento extra, o sea, un crecimiento del catorce por ciento, catorce punto ocho o catorce punto nueve por ciento aproximadamente."
   **Anotación del agente:** Correcto y bien razonado — distinguió explícitamente el crecimiento TOTAL (1.32) de la tasa ANUAL equivalente (√1.32≈1.1489), y explicó por qué la raíz es la operación correcta (aplicar el operador dos veces debe reproducir 1.32).

5. **Enunciado:** Precio medio de mezcla: 3 kg de café a S/40/kg + 5 kg a S/28/kg.
   **Respuesta del usuario (textual):** "cuarenta por tres [...] veintiocho [...] por cinco [...] ciento veinte más ciento cuarenta, eso va a ser doscientos sesenta [...] tenemos cinco kilos. Entonces, sería doscientos sesenta entre cinco, Que nos deja a cincuenta y dos soles el kilo."
   **Anotación del agente:** Error identificado en `dificultades.json` (dif-10-01): dividió entre 5 (cantidad de un solo componente) en vez de 8 (total). Se le preguntó "¿cuántos kilos hay en total?" y se autocorrigió de inmediato: "debió haber sido doscientos sesenta entre ocho [...] treinta y dos, cincuenta el kilo." Correcto tras la corrección.

6. **Enunciado:** Ley de aleación: 6 g de oro ley 18/24 + 4 g de oro ley 12/24.
   **Respuesta del usuario (textual):** "para el primero [...] cuatro coma cinco gramos. Y para el segundo [...] dos gramos [...] seis coma cinco gramos de oro puro [...] dividirlo entre el peso total, que el peso total de seis más cuatro, diez gramos [...] cero coma sesenta y cinco [...] multiplico por veinticuatro arriba y por veinticuatro abajo [...] quince coma seis quilates."
   **Anotación del agente:** Correcto de principio a fin, sin errores ni ayuda — incluyendo la conversión final a quilates.

7. **Enunciado (aplicación libre del usuario):** Verificar MH con gasto fijo: S/100 en gasolina dos veces, a S/4/L y S/5/L, ¿precio promedio real?
   **Respuesta del usuario (textual):** "fui dos veces a la gasolinería [...] pagué cien soles [...] cien entre cuatro [...] veinticinco litros [...] estaba cinco soles, entonces eran veinte litros [...] compré en total veinticinco más veinte cuarenta y cinco litros [...] pagué doscientos soles [...] Doscientos entre cuarenta y cinco [...] a cuatro punto cuatro cuatro periódico puro. Sí, es diferente al cuatro punto cinco [...] Qué es lo que se mantuvo constante el gasto, los doscientos soles [...] Qué es lo que cambió? Cambió los precios, cambió la cantidad de litros comprada."
   **Anotación del agente:** Resuelto correctamente por lógica pura (sin aplicar la fórmula de MH conscientemente), y luego identificó correctamente qué cantidad se preservaba (el gasto total) y qué variaba. Este ejercicio cerró la laguna del Feynman (ver dif-10-02).

## Herramientas usadas
Ninguna externa — todo el cálculo se hizo mentalmente/verbal, cifras simples a propósito para poder verificar a mano.

## Dudas / puntos flojos
Ver `dificultades.json`: dif-10-01 (división por cantidad parcial en vez del total, autocorregido) y dif-10-02 (confundir el escenario de aplicación de MH con el de mezclas, en el primer intento del Feynman — resuelto en la misma sesión).

## Feynman de cierre
**Consulta del agente (textual):** "Explícame el tema de hoy (promedios, mezclas y aleaciones) como si se lo estuvieras enseñando a alguien que no sabe nada del tema. Qué es cada uno de los tres promedios, cuándo se usa cada uno, y cómo se conecta todo con la idea de 'promedio ponderado' de la que partimos."

**Respuesta del usuario (primer intento, textual, resumido por espacio — ver mensaje completo en el historial de chat):** Explicó MA con una analogía confusa de "compañeros distribuidos en un espacio" pero la mecánica (fórmula, preserva la suma) correcta. Explicó MG con el ejemplo de zapatillas subiendo de precio día a día, con la mecánica correcta pero el propósito algo confuso (predecir un día específico en vez de hallar una tasa equivalente). Para MH usó el ejemplo de la mezcla de café/harina/azúcar — **error real**: ese escenario es promedio ponderado, no armónico.

**Anotación del agente:** En vez de corregir directo, se le devolvió su propia respuesta correcta de minutos antes en la conversación ("MA preserva la suma... MH preserva la suma de los pesos [tiempo]... MG preserva la multiplicación de los pesos") y se le pidió reconstruir "qué se mantiene igual" para cada uno en una frase. Lo logró correctamente los tres. La laguna real no era conceptual sino de **articulación espontánea** (Feynman sin apoyo) — se cerró con el ejercicio 7 (gasolina), que resolvió y conectó correctamente sin ayuda.

**Reflexión propia del usuario (textual):** "me choco un poco [...] cómo puedo resolver algo si no sé qué es lo estoy resolviendo" — expresó incomodidad por resolver por lógica pura sin reconocer el tipo de problema de inmediato.
**Anotación del agente:** Se le explicó que reconocer el patrón instantáneamente es criterio de nivel `avanzado` (rúbrica del curso), no de `base` — hoy solo se verifica `base`, que explícitamente permite apoyarse en el razonamiento/teoría escrita. Resolver por estructura antes que por reconocimiento de nombre es la base más sólida, no una debilidad.

## Próximos pasos
Siguiente tema en ruta: `11-interes-simple`. Sin dificultades abiertas pendientes de este tema. Vigilancia pasiva sugerida: la próxima vez que aparezca un problema de mezclas o promedios, comprobar que ya no confunde el escenario de MH con el de mezcla ponderada (dif-10-02) y que sigue sumando el total de cantidades antes de dividir (dif-10-01, mismo patrón que dif-08-01 en dos temas distintos — vale la pena una nota general sobre "sumar antes de dividir" si reaparece una tercera vez).
