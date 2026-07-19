# Prompt de porteo al TEMPLATE de origen

> **Archivo temporal.** Es un prompt autocontenido para copiar y pegar en una sesión de agente abierta **en el repo del TEMPLATE genérico** (el que trae `_protocolos/`, `_plantillas/`, el algoritmo de arranque y el `_ejemplo-algebra/`). No es parte del sistema de estudio de este repo; bórralo cuando el porteo esté hecho.
>
> Contexto: este repo (`estudy-Han`) nació de ese template. Entre el 2026-07-16 y hoy se rediseñó el núcleo del proyecto. **Todo el rediseño es mejora de ARQUITECTURA, no contenido personal**, así que debe volver al template para que cualquier usuario nuevo lo tenga. Este prompt explica cada cambio **con su porqué** (los "por qué" son lo valioso: son las decisiones de diseño, no las mecánicas).

---

## PEGA ESTO EN LA SESIÓN DEL TEMPLATE

Estás trabajando en el repositorio del TEMPLATE genérico de "plan de estudios asistido por IA". Un repo derivado de este template evolucionó su arquitectura y esos cambios deben volver aquí, al template, para todos los usuarios futuros. NO vas a portar ningún contenido personal (cursos, objetivos reales, historial): solo la arquitectura, las plantillas, los protocolos y las reglas.

**Antes de tocar nada:** lee por completo el `README.md`, los `_protocolos/*.md` y los `_plantillas/*` actuales del template para saber de qué punto partes. Muchos cambios son reescrituras de secciones existentes, no añadidos.

### El cambio de fondo: el conocimiento se mide en DOS dimensiones

El modelo viejo medía un tema con un solo eje difuso ("¿está completado?", a veces con un `porcentaje` 0-100). El modelo nuevo lo mide con **dos coordenadas ortogonales**, y ninguna es "completado":

- **Profundidad** → `nivel_alcanzado` en `progreso.json`: `null → base → intermedio → avanzado`. Es un **estado VERIFICABLE**: solo sube cuando el usuario cumple el `criterio_dominio` de ese nivel. **Nunca sube por haber gastado sesiones.**
- **Durabilidad** → `nivel_repaso` (0-5): cuánto va a aguantar antes de olvidarse (1≈1 día, 2≈3, 3≈7, 4≈14, 5≈30).

**Por qué:** un tema puede estar aprendido hondo y sin repasar (se desvanece) o superficial y muy repasado (aguanta pero es superficial). Un solo eje no puede decir si un tema estará vivo Y suficiente el día del examen. Y "completado" es una pregunta mal planteada: **¿completado para quién?** Factorización puede estar terminada para aprobar un semestre y lejísimos de lo que exige un examen de admisión. Por eso la doneness es **derivada y relativa**: un tema está completo *para un objetivo* si `nivel_alcanzado >= nivel_requerido` por ese objetivo.

### Lista de cambios a portar, cada uno con su porqué

1. **Matar el campo `porcentaje` de `progreso.json` y de su plantilla.** Duplicaba mal a `nivel_alcanzado`: dos campos midiendo lo mismo con distinta precisión = deriva garantizada. La plantilla de progreso queda con `estado`, `nivel_alcanzado`, `ultima_sesion`, `sesiones_dedicadas`, `nivel_repaso`, `fecha_proximo_repaso`.

2. **`estado` es FLUJO puro, tres valores:** `no_iniciado` / `en_progreso` / `completado`. Cuidado con la palabra: `completado` aquí significa **"las clases activas sobre el tema terminaron y pasó a la escalera de repaso"**, NO una medida de conocimiento. Un tema puede quedar `completado` con `nivel_alcanzado: base`. **Por qué se conserva el tercer valor:** el selector de clase (PASO 5.5) necesita distinguir "arráncalo" (no_iniciado) de "retómalo" (en_progreso) de "déjalo, ya está en mantenimiento" (completado); con solo dos valores, la rama de tema nuevo nunca dispararía.

3. **`nivel_alcanzado` tiene UN solo escritor: el "Protocolo al CERRAR una clase".** Reescribe ese protocolo para que: (a) averigüe el `nivel_objetivo_hoy` = mayor `nivel_requerido` del tema entre los objetivos activos; (b) lea el `criterio_dominio` del nivel a evaluar; (c) verifique contra la evidencia REAL de la clase (ejercicios + Feynman), subiendo `nivel_alcanzado` un nivel a la vez y **deteniéndose en el primer nivel no cumplido**; (d) si la clase se cortó antes de verificar, `nivel_alcanzado` NO sube y `estado` queda `en_progreso`. **Por qué:** un campo central sin escritor es decoración. Y el esfuerzo no es verificable pero el estado sí — un contador de sesiones diría "pagado, siguiente" y mentiría.

4. **Escalera de repaso atada a la PROFUNDIDAD, no a `estado`.** Se arma (`nivel_repaso: 1`, `fecha_proximo_repaso` = mañana) en cuanto el tema alcanza por primera vez un `nivel_alcanzado` no nulo. **Por qué:** un tema en `base` ya empieza a olvidarse y necesita mantenimiento, esté `en_progreso` o `completado`.

5. **Escalera ADAPTATIVA al hito, no fija.** El `nivel_repaso` objetivo de un tema es el menor nivel cuya retención cubre la distancia de hoy al hito no condicional más cercano que lo incluye (tope 5). Hito lejano → hay que subir a 5; hito cercano → la escalera se trunca sola. **Por qué:** cada peldaño de más es un repaso que le robas a otro tema; y un tema cerrado a 4 semanas del examen no tiene sitio físico para más de 2-3 repasos.

6. **El coste de MANTENER cuesta horas y hay que contarlas.** En todas las validaciones de viabilidad (entrevista, expansión, comando `estado`, regla 10 del checklist): `horas_necesarias = horas_aprender + horas_mantener`, donde `horas_mantener = (repasos que la escalera dispara hasta el hito) × minutos_por_repaso`. **Por qué:** el modelo viejo contaba solo el aprendizaje; con muchos temas, la escalera dispara cientos de repasos (decenas de horas) que el plan no veía, y decía "cabe" cuando no cabía. Ojo: el 70/30 del intercalado y la escalera son el MISMO fenómeno — no los cuentes dos veces.

7. **Predicción de olvido, obligatoria.** Al planificar un hito, avisa de los temas que llegarán FRÍOS (su `nivel_repaso` no retiene la distancia hasta la fecha). Debe estar en el comando `estado`, en el PASO 5.5 (canario) y en las validaciones de entrevista/expansión. **Por qué:** es lo único que distingue un plan que cuenta horas de uno que predice resultados. No se omite por no dar malas noticias.

8. **`HORAS_POR_SESION` y `minutos_por_repaso` se MIDEN, no se suponen.** Se calculan desde un `historial.json` append-only (una entrada por sesión: `inicio`, `duracion_min` —preguntado al usuario, no estimado—, `tipo_clase`, `objetivo_id`, `curso`, `tema_principal`, `temas_intercalados`). `minutos_por_repaso` sale de las sesiones `tipo_clase: "repaso"`. Sin datos, usa 1.5 h/sesión y 12 min/repaso **declarándolo como suposición**. Añade `historial.json` y su plantilla al template. **Por qué:** una viabilidad calculada sobre una constante inventada, presentada como hecho, es la forma más fácil de que un plan nazca mintiendo.

9. **Objetivos paralelos: `objetivo.json` tiene un ARRAY `objetivos`**, cada uno con `estado` (`activo` / `latente` / `completado`), `prioridad` ordinal (1 = lo primero, gana el menor), `condicion_activacion` (aritmética verificable, para los latentes), `hitos` (array de fechas de corte, con `condicional: true` para sustitutorios) y `cursos_requeridos`. **Por qué:** un usuario puede perseguir varias metas a la vez con ciclos de vida distintos (un semestre, un examen de admisión, un idioma). La máquina de estados reordena sola: cuando el objetivo de prioridad 1 se completa, el 2 sube sin tocar números.

10. **`cursos_requeridos` es muchos-a-muchos con NIVEL por curso.** Cada entrada: `{ curso, nivel_requerido, overrides }`, donde `overrides` sube/baja temas concretos. Un curso puede estar en varios objetivos a niveles distintos. **Por qué (bug real):** el modelo viejo daba a cada curso un "dueño" único del que heredaba fecha y prioridad; un curso base compartido (trigonometría) heredaba la fecha lejana del objetivo que lo creó y se posponía justo cuando otro objetivo lo necesitaba YA. Un campo respondía dos preguntas ("¿quién lo creó?" y "¿quién lo necesita y para cuándo?"); solo la segunda importa. Elimina el `liston` por objetivo (`dominar`/`pasar`): era una vara única y gruesa; el `nivel_requerido` por curso con overrides es la versión con grano.

11. **`presupuesto_horas` es un ARRAY de periodos, no un escalar.** `[{ desde, hasta, horas_semanales, nota }]`; las horas hasta una fecha se INTEGRAN sobre los periodos que se solapan. **Por qué:** la disponibilidad del usuario cambia (vacaciones vs. semestre vs. trabajo). Un escalar promedia dos mundos distintos y miente en ambas direcciones. Elimina cualquier `horas_semanales_disponibles` escalar y cualquier `horas_semanales` por objetivo (los objetivos NO reparten el presupuesto: compiten por él en una cola por prioridad, porque con cursos compartidos "¿esta hora sale del presupuesto de qué objetivo?" no tiene respuesta).

12. **La viabilidad se calcula como una COLA CON PRIORIDAD que cobra cada curso una sola vez.** Recorre los objetivos por prioridad y, dentro, sus hitos por fecha; usa `proyectado[tema]` (el nivel al que lo dejarán los objetivos ya procesados) para cobrar solo los SALTOS de nivel que falten; descuenta `horas_comprometidas` por los más urgentes. La **cerradura transitiva** arrastra prerrequisitos internos y externos (`"curso/tema_id"`), parando en lo ya suficiente para ese objetivo (`proyectado >= nivel_requerido`). **Por qué:** modela la competencia real por horas y elimina el doble conteo sin trucos; el objetivo más urgente reserva primero y el siguiente hereda lo hecho. Esto reemplaza el viejo cobro por diferencia medido en horas (`ya_cobrados`/`pagado`), que sobrevive como concepto pero ahora sobre un estado verificable (niveles) en vez de horas gastadas.

13. **Prerrequisitos SIEMPRE a nivel de TEMA, nunca de curso** (`prerrequisitos_internos` por id, `prerrequisitos_externos` como `"curso/tema_id"`). **Por qué:** "Estática necesita geometría" es cierto e inútil; lo que necesita es `03-geometria/09-areas` para momentos de inercia, no las cónicas. Una dependencia a nivel de curso arrastra el curso entero e infla el plan hasta verse imposible.

14. **`modo_estudio` obligatorio en cada `temario.json`** (`ruta` / `cadencia` / `hito`), con condiciones en **aritmética verificable**. Y el **PASO 5.5 (selector de clase)** completo, con sus ramas (A) continuación, (B) hito urgente, (C) repaso, (D) cadencia, (E) hito normal, (F) tema nuevo, más las reglas (propón-no-impongas, no hardcodear cursos, el CANARIO). **Por qué:** un curso sin `modo_estudio` queda huérfano y nunca se estudia — el bug que este proyecto tuvo hasta julio de 2026, agravado porque nada lo hacía visible. El CANARIO y el bloque de alertas del comando `estado` son la red que hace que "un disparador que no dispara" se vea.

15. **`rubrica_niveles` por curso en cada `temario.json`.** Define `base`/`intermedio`/`avanzado` **como CAPACIDADES, nunca como el puntaje en el examen de un tercero.** El `criterio_dominio` específico por tema/nivel se DERIVA de la rúbrica pero **no se fabrica por adelantado**: se escribe cuando el tema se estudia. **Por qué:** anclar la vara a "8/10 tipo UNI" ata la medición a una entidad externa que solo está calibrada para algunos cursos (sirve para matemática, no para razonamiento verbal ni para un idioma).

16. **`una_clase_por_sesion`** (antes `un_tema_por_sesion`) y la "Anatomía de una clase" (sesión = un chat; clase = la unidad del día, con como máximo un tema nuevo + repaso intercalado). **Por qué del renombre:** el nombre viejo chocaba con el Aprendizaje Intercalado (Método 3), que pide mezclar ejercicios de varios temas en una clase; la unidad correcta es la clase, no el tema. Incluye la "Contención del alcance de la clase" (la clase que crece sola por curiosidad).

17. **Handoff de ida y vuelta al modelo de niveles.** El prompt de SALIDA transmite el nivel objetivo, el criterio por nivel y el nivel ya verificado; el resumen de VUELTA trae **"Nivel demostrado" + evidencia** (no un porcentaje), y la fase de VUELTA escribe `nivel_alcanzado` con la misma regla verificable que el cierre (subiendo con cautela, sin bajar, pidiendo confirmación si duda de la verificación externa). **Por qué:** una sesión externa tiene que poder decir "llegó a base pero no a intermedio", no solo "avanzó un %".

18. **Reglas de calidad del temario** (checklist): en particular la **regla 4** (prerrequisitos a nivel de tema), la **regla 11** (`modo_estudio` obligatorio) y la **regla 12** (el grafo crece desde la necesidad hacia atrás, parando en lo ya sabido — un curso creado para dar soporte a otro puede tener tres temas y estar perfecto).

19. **CABLEADO DE LAS ALERTAS: invocador automático, no manual** (añadido 2026-07-18 — **el más importante de esta lista para una sesión fría**, y el que sustituye al "CANARIO" que mencionan los items 7 y 14). Porta las cuatro piezas juntas, no sueltas:
    - **Clasificar cada alerta por coste** en el bloque de alertas de `utilidades.md`: **BARATAS** (solo leen JSON + aritmética directa) vs **CARAS** (exigen la cola de viabilidad). Regla de default: **una alerta sin coste declarado se trata como BARATA**, porque el fallo caro siempre es el detector que no dispara.
    - **La regla 6 del PASO 5.5 corre TODAS las BARATAS en cada arranque**, automáticamente, y **apunta al bloque de alertas en vez de copiarlo** (dos copias se desincronizan y se lee la vieja). Reporta todas las que se cumplan aunque otra rama haya ganado y aunque no tengan que ver con la clase de hoy.
    - **Checkpoint fechado** `objetivo.json → ultima_auditoria` (`fecha`, `vence_a_los_dias`), cuyo **único escritor es el comando `estado`**, más una alerta BARATA que avisa cuando vence. Hace que **no correr lo caro sea visible** en vez de silencioso.
    - **Item nuevo en el checklist de Capa 2A de `capas.md`:** toda regla declara si su invocador **corre solo o depende de que alguien se acuerde**; un invocador manual **NO cuenta como cableado**. Y declara **en qué momento hace falta**, verificando que el protocolo que corre en ese momento la invoque.

    **Por qué (fallo real, 2026-07-18):** este proyecto tenía 14 alertas bien escritas —incluida una que detectaba exactamente el problema que se estaba produciendo— y **las 14 dependían de que el usuario escribiera `estado` a mano.** En una sesión fría eso no ocurre. El agente propuso una clase de un objetivo secundario teniendo leído el déficit de 52 h del prioritario, y el usuario tuvo que sacárselo a la fuerza en dos mensajes. **La lección general: leer los protocolos no basta si la regla que hace falta vive en el protocolo de OTRA acción.** Ese fallo no se arregla leyendo más — se arregla cableando. Un template que porte las alertas sin su cableado automático hereda el bug entero con apariencia de estar completo.

    **Corolario para el ruido:** al cablear alertas al arranque, una alerta que se cumple para muchos cursos a la vez debe **agregarse en una línea**, no enumerarse. Doce líneas entierran a la que importaba y a las dos semanas el usuario salta el bloque — que es cómo una red de seguridad se apaga sola sin que nadie la apague.

20. **EL SELECTOR (PASO 5.5) TRABAJA CON DOS CONJUNTOS, NO CON `cursos_requeridos`** (añadido 2026-07-18, patrón P9). `cursos_requeridos` es solo el **tronco** — los cursos de los que el objetivo *trata*. El árbol real sale de la cerradura. Porta las tres cosas:
    - **`aprender(objetivo)` = tronco ∪ `cerradura(semilla, objetivo)`** — usando la MISMA función `cerradura()` de la viabilidad, no una copia. Corta en lo que ya está a nivel suficiente. La usan la rama (F) y la viabilidad.
    - **`mantener(objetivo)` = todos los temas del árbol de dependencias CON escalera armada, SIN cortar por nivel alcanzado.** La usan la rama (C) y la lista de intercalado.
    - **La urgencia efectiva se calcula POR TEMA, no por curso**, y un tema puede entrar por el tronco o por la cerradura.
    - La rama (F) comprueba `prerrequisitos_externos` **al elegir**, no después.

    **Por qué (dos bugs encadenados, el mismo día):** *(a)* con el selector leyendo solo `cursos_requeridos`, un objetivo que lista únicamente sus cursos "propios" deja sus prerrequisitos base heredando la urgencia de OTRO objetivo —el que sí los lista— y se posponen justo cuando son urgentes. Peor: si ese otro objetivo pasa a `latente`, desaparecen del selector por completo. *(b)* Al arreglar (a) usando `cerradura()` a secas apareció el segundo: la cerradura **corta en lo ya sabido** —correcto para "¿qué me falta aprender?" y exactamente lo contrario de lo que necesita el repaso, donde lo ya sabido ES el objeto—. Eso dejaba fuera del radar de repaso los temas ya estudiados de los que el objetivo prioritario depende, y se habrían desvanecido en silencio. **Un arreglo destapó el otro: si portas solo `aprender()`, portas el segundo bug.**

### Qué NO portar (contenido personal, no arquitectura)

- Ninguno de los cursos reales (`01-...` a `13-...`): sus `temario.json`, `progreso.json`, `glosario.json`, `dificultades.json`, `clases/`.
- El `objetivo.json` con las metas reales del usuario (UNI, semestre UNFV, etc.).
- El `historial.json` con sesiones reales.
- El template conserva su `_ejemplo-algebra/` como única muestra de curso; no lo reemplaces con cursos reales.

### Cómo trabajar

- Revisa el historial de git del repo derivado (`git log` desde `cb25da7` en adelante, y el commit "Snapshot rediseño dos dimensiones") para no olvidar ningún cambio: los mensajes de commit resumen cada bloque.
- Aplica los cambios como reescrituras de las secciones existentes del `README.md` y los protocolos, no como apéndices.
- Al terminar, verifica que las plantillas (`plantilla-objetivo.json`, `plantilla-temario.json`, `plantilla-progreso.json`, `plantilla-historial.json`) reflejen el modelo nuevo, y que el `_ejemplo-algebra/` siga siendo coherente con él (o actualízalo).
- El template NO lleva `PENDIENTE.md` ni el aviso de migración: esos eran del repo derivado.
