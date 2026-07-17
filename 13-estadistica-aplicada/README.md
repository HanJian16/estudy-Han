# Estadística Aplicada (UNFV, IV ciclo)

> **Curso universitario real**, no de preparación. Código 32 del plan curricular 2019 de Ing. Mecatrónica, 3 créditos, 16 semanas. Sirve al objetivo `obj-2` (aprobar el 4to semestre).

## Prerrequisitos externos

> La verdad operativa está en los `prerrequisitos_externos` de cada tema del `temario.json`. Esto es el resumen para humanos.

De la malla UNFV: **Estadística** (código 25, III ciclo) — ya aprobada por el usuario. Cubrió **toda la descriptiva**, correlación y regresión lineal simple, y probabilidad **solo como introducción**.

De los cursos de base de este proyecto, es el que **menos base arrastra de los cinco**:

- `01-aritmetica/12-estadistica` — la descriptiva preuniversitaria. Ya está `completado`... no: pendiente. Es el andamio.
- `01-aritmetica/10-promedios-mezclas` — media y media ponderada.
- `02-algebra/08-binomio-newton` — los coeficientes binomiales *son* la distribución binomial.
- `02-algebra/14-funciones` — funciones de densidad.
- `02-algebra/17-matrices-determinantes` — regresión múltiple en forma matricial.

## Objetivo del curso

Aprobar el parcial (**28.09.2026**) y el final (**23.11.2026**). El listón: **bases suficientes para dar cara**, no dominio.

El curso enseña a **decidir con datos bajo incertidumbre**. Su bloque de control estadístico de calidad reaparece en Ingeniería de Procesos, manufactura y automatización.

## Reparto por hito

| Hito | Fecha | Temas |
|---|---|---|
| `parcial` | 28.09.2026 | 01-05: distribuciones discretas y continuas, muestrales y TCL, estimación, pruebas de una muestra |
| `final` | 23.11.2026 | 06-11: pruebas de dos muestras, chi-cuadrado, ANOVA, regresión múltiple, control de calidad, diseño de experimentos |

## ⚠ Temario inferido — el hueco está muy bien delimitado

El sílabo oficial **no está publicado**, pero aquí el pan de abajo es preciso. El sílabo *real* de Estadística (ciclo 3, docente MSc. Alberto Alvarado Salazar) tiene cuatro unidades: conceptos y tablas de frecuencias, medidas de resumen, correlación/series de tiempo/regresión simple, e **introducción** a probabilidad y variables aleatorias. Su sumilla lo dice: *"estadística descriptiva y de probabilidades"*.

**No hay inferencia, ni intervalos de confianza, ni pruebas de hipótesis, ni ANOVA.** Ese es exactamente el hueco que Estadística Aplicada tiene que llenar, y es lo que la ingeniería necesita.

Además, **Montgomery ya está citado como fuente en el sílabo real del prerrequisito**. Su *Probabilidad y Estadística Aplicada a la Ingeniería* es literalmente este curso, así que la bibliografía inferida tiene base real y no es un tiro al aire.

**Revisar en la semana 1 de clases (10-14 de agosto de 2026).** Lo más probable que cambie: cuánto entra de diseño de experimentos (por eso el tema 11 va `opcional: true`) y qué software se usa. Lo menos probable: que falten pruebas de hipótesis e intervalos de confianza.

## Notas generales

**El salto real del curso es `03-distribuciones-muestrales-tcl`.** Ahí se pasa de describir datos que tienes a inferir sobre una población que no ves, y todo lo demás descansa en el teorema central del límite. Si eso queda flojo, el resto se vuelve receta memorizada: el usuario aplicará fórmulas sin saber por qué. Insiste con interrogación elaborativa antes de avanzar.

**El error más caro de este curso no es de cálculo: es plantear mal H0 y H1**, y leer mal la tabla (cola equivocada, grados de libertad equivocados). Trátalo como dificultad `aplicacion` y `notacion`, no `conceptual`.

**Es el curso más independiente de los cinco** — no lo necesita ningún otro del semestre y arrastra poca base. Por eso lleva `prioridad: 3`: si una semana aprieta, es el primer candidato a ceder. Pero tiene examen igual que los demás, así que ceder no es lo mismo que abandonar.

Es también, junto con el control de calidad, el bloque más procedimental del semestre: **se aprueba practicando**, no entendiendo más.
