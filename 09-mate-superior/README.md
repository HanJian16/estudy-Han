# Matemática Superior (UNFV, IV ciclo)

> **Curso universitario real**, no de preparación. Código 26 del plan curricular 2019 de Ing. Mecatrónica, 4 créditos, 16 semanas. Sirve al objetivo `obj-2` (aprobar el 4to semestre).

## Prerrequisitos externos

> La verdad operativa está en los `prerrequisitos_externos` de cada tema del `temario.json`, a nivel de tema. Esto es solo el resumen para humanos.

De la malla UNFV: **Matemática Aplicada** (código 18, III ciclo) — ya aprobada por el usuario.

De los cursos de base de este proyecto, este curso arrastra **solo estos temas**, no los cursos enteros:

- `02-algebra/05-factorizacion` y `02-algebra/06-fracciones-algebraicas` — **el bloque más crítico**. Las fracciones parciales *son* el método de la transformada inversa de Laplace; sin eso, el 40% del parcial es inaccesible.
- `02-algebra/09-numeros-complejos` — base de variable compleja y de la transformada de Fourier.
- `02-algebra/14-funciones`, `02-algebra/15-logaritmos` — exponenciales para las transformadas directas.
- `02-algebra/12-sistemas-ecuaciones` — para sistemas de EDOs por Laplace.
- `04-trigonometria/08-identidades-fundamentales`, `04-trigonometria/12-funciones-trigonometricas` — Fourier es literalmente senos y cosenos.

## Objetivo del curso

Aprobar el parcial (**28.09.2026**) y el final (**23.11.2026**). El listón lo fijó el usuario: **bases suficientes para dar cara en los exámenes**, no dominio. Calibra los ejercicios a eso.

Dicho esto, Laplace y Fourier no son un trámite: son la herramienta central de Procesamiento Digital de Señales y Control I/II, que vienen detrás en la malla. Lo que se haga bien aquí se cobra tres veces.

## Reparto por hito

El final **no es acumulativo** (confirmado en el sílabo real del prerrequisito, que separa "EXAMEN PARCIAL DE LA UNIDAD 01 Y 02" del examen final). Cada tema declara su `hito_id`:

| Hito | Fecha | Temas |
|---|---|---|
| `parcial` | 28.09.2026 | 01-06: toda la transformada de Laplace + entrada a series de Fourier |
| `final` | 23.11.2026 | 07-12: Fourier (simetría y transformada), EDPs y variable compleja |

## ⚠ Este temario es INFERIDO

El sílabo oficial **no está publicado** (figura "En proceso" en la web de la EPIM). Este temario se construyó por inferencia por sándwich:

- **Pan de abajo, sólido:** el sílabo *real* de Matemática Aplicada (código 100957) dice exactamente dónde termina el alumno — funciones de varias variables, integrales múltiples, EDOs de primer orden y de orden superior. **No cubre Laplace, Fourier, EDPs ni variable compleja.**
- **Pan de arriba, débil:** Matemática Superior es prerrequisito de Ingeniería de Procesos, y detrás vienen Procesamiento Digital de Señales y Control I/II, que necesitan Laplace y Fourier. Pero **sus sílabos tampoco están publicados**, así que ese lado se apoya en el grafo de la malla y en el estándar de la disciplina, no en un documento.
- El usuario confirmó (2026-07-16) que el contenido inferido encaja con lo que le han dicho.

**Acción obligatoria: revisar este temario en la semana 1 de clases (10-14 de agosto de 2026)**, cuando el profesor presente el sílabo real. Lo más probable que cambie: el orden de las unidades y el corte exacto parcial/final. Lo menos probable: que Laplace y Fourier no estén.

## Notas generales

Curso operativo: se aprueba resolviendo, no leyendo. Las tablas de transformadas se interiorizan a mano — el usuario debe llegar al examen sin depender de consultarlas para los casos frecuentes.

El punto de fricción previsible es `03-laplace-inversa-fracciones-parciales`: no por Laplace, sino por el álgebra de fracciones parciales, que el usuario tiene oxidada. Si ahí se atasca, el problema está en `02-algebra`, no en este curso — trátalo como una dificultad de base y no como un fallo del tema.
