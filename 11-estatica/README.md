# Estática (UNFV, IV ciclo)

> **Curso universitario real**, no de preparación. Código 29 del plan curricular 2019 de Ing. Mecatrónica, 3 créditos, 16 semanas. Sirve al objetivo `obj-2` (aprobar el 4to semestre).

## Prerrequisitos externos

> La verdad operativa está en los `prerrequisitos_externos` de cada tema del `temario.json`. Esto es el resumen para humanos.

De la malla UNFV: **Física II** (código 19) — pero ojo, eso es una **compuerta administrativa, no de contenido**. Física II es elasticidad, ondas, fluidos y termodinámica. El fundamento real de Estática es Física I (mecánica y vectores), del ciclo 2, ya aprobada por el usuario.

De los cursos de base de este proyecto:

- `05-fisica/02-vectores` — **el más crítico de todo el curso**. Estática *es* álgebra vectorial aplicada. Si esto falla, no falla un tema: falla el curso entero.
- `05-fisica/06-estatica` — la versión preuniversitaria (DCL, primera y segunda condición de equilibrio, Lamy). Es el andamio conceptual.
- `04-trigonometria/03-razones-angulos-agudos`, `04-trigonometria/04-triangulos-rectangulos-angulos-verticales`, `04-trigonometria/15-triangulos-oblicuangulos` — descomposición de fuerzas y leyes de senos/cosenos.
- `03-geometria/09-areas` y `03-geometria/06-puntos-notables` — centroides y centro de gravedad.
- `03-geometria/02-triangulos` — geometría de armaduras.
- `02-algebra/12-sistemas-ecuaciones` — el equilibrio *es* un sistema de ecuaciones.

## Objetivo del curso

Aprobar el parcial (**28.09.2026**) y el final (**23.11.2026**). El listón: **bases suficientes para dar cara**, no dominio.

Dicho eso, **es el curso más rentable del semestre**. Alimenta a Resistencia de Materiales (que se cursa en paralelo) y es prerrequisito de Dinámica, Diseño de Máquinas y Elementos de Máquinas. Lo que se haga bien aquí se cobra durante media carrera.

## ⚠ Paralelo con Resistencia de Materiales, no anterior

Estática y Resistencia son **ambos del IV ciclo**: se cursan a la vez, no uno detrás de otro. Formalmente ninguno depende del otro (los dos tienen Física II como requisito).

**Pero en contenido Resistencia sí depende de Estática**, en dos puntos concretos:

- `07-fuerzas-internas-cortante-momento` — sin diagramas de cortante y momento flector, Resistencia es imposible.
- `10-momentos-inercia` — el momento de inercia de la sección es lo que determina cuánto flexiona una viga.

**Consecuencia práctica: hay que estudiar Estática ligeramente por delante de Resistencia**, y adelantar los temas 07 y 10 si Resistencia los pide antes de lo que dicta el calendario. Por eso este curso lleva `prioridad: 1`: si se atrasa, arrastra dos cursos en vez de uno.

## Reparto por hito

| Hito | Fecha | Temas |
|---|---|---|
| `parcial` | 28.09.2026 | 01-05: vectores, equilibrio de partícula, momentos, equilibrio de cuerpo rígido, armaduras |
| `final` | 23.11.2026 | 06-11: marcos, fuerzas internas, fricción, centroides, momentos de inercia, trabajo virtual |

## ⚠ Este temario es INFERIDO — y con el ancla más débil de los tres

El sílabo oficial **no está publicado**. Y aquí el sándwich funciona distinto que en Matemática Superior o Física III:

- **El pan de abajo NO sostiene.** El prerrequisito formal (Física II) sí tiene sílabo real, pero no contiene el fundamento de Estática. El fundamento es Física I, del ciclo 2, **cuyo sílabo no está publicado** — solo existen los de ciclos 1 y 3.
- **Lo que sostiene es otra cosa: el estándar de la disciplina.** Estática es probablemente el curso más estandarizado de la ingeniería. Hibbeler y Beer & Johnston lo cuentan igual en todo el mundo y su orden de unidades apenas varía. La confianza aquí no viene de un documento, viene de eso.
- Estática es prerrequisito de Dinámica (V ciclo), lo que confirma que cubre el bloque clásico completo.

**Es el curso donde más urge conseguir el sílabo real**, precisamente porque su ancla documental es la más floja. Revisar en la semana 1 de clases (10-14 de agosto de 2026). Lo más probable que cambie: si entra trabajo virtual (por eso el tema 11 va `opcional: true`) y el corte parcial/final. Lo menos probable: que falten centroides, momentos de inercia o fuerzas internas — sin eso Resistencia sería imposible.

## Notas generales

**Estática es el diagrama de cuerpo libre y nada más.** Un DCL mal hecho no se arregla con álgebra; uno bien hecho casi resuelve el problema solo. Si el usuario se salta el dibujo, párale — ahí está el 90% de los errores de este curso, no en las cuentas.

El otro punto de fricción previsible es el signo de los momentos y el sentido de las reacciones. No es falta de comprensión: es descuido operativo. Trátalo como dificultad `operativa`, no `conceptual`.
