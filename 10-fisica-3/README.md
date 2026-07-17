# Física III (UNFV, IV ciclo)

> **Curso universitario real**, no de preparación. Código 27 del plan curricular 2019 de Ing. Mecatrónica, 4 créditos, 16 semanas. Sirve al objetivo `obj-2` (aprobar el 4to semestre).

## Prerrequisitos externos

> La verdad operativa está en los `prerrequisitos_externos` de cada tema del `temario.json`, a nivel de tema. Esto es solo el resumen para humanos.

De la malla UNFV: **Física II** (código 19, III ciclo) — ya aprobada por el usuario. Cubrió elasticidad, oscilaciones, ondas, fluidos y termodinámica; **nada de electromagnetismo**.

De los cursos de base de este proyecto, arrastra **solo estos temas**:

- `05-fisica/13-electrostatica`, `05-fisica/14-electrodinamica`, `05-fisica/15-electromagnetismo` — la versión preuniversitaria de lo mismo. Es el andamio conceptual: llegar a la versión universitaria sin esto es empezar a ciegas.
- `05-fisica/02-vectores` — **el más crítico**. Los campos son vectoriales y aquí todo es producto punto y producto cruz. Sin vectores no hay Física III.
- `05-fisica/05-movimiento-circular` — para partículas cargadas en campo magnético.
- `05-fisica/10-mas-ondas` — para ondas electromagnéticas.
- `02-algebra/12-sistemas-ecuaciones` — las mallas de Kirchhoff son sistemas de ecuaciones y nada más.

## Objetivo del curso

Aprobar el parcial (**28.09.2026**) y el final (**23.11.2026**). El listón lo fijó el usuario: **bases suficientes para dar cara**, no dominio.

Dicho eso, este curso es la base física de casi toda la especialidad: Circuitos Eléctricos I y II, Máquinas Eléctricas, Electrónica y Control tiran de aquí.

## Reparto por hito

El final **no es acumulativo** — confirmado en el sílabo real de Física II, que dice literalmente *"EXAMEN PARCIAL: Evaluación correspondiente a la Unidad N° I, II y III"* en la semana 8.

| Hito | Fecha | Temas |
|---|---|---|
| `parcial` | 28.09.2026 | 01-06: electrostática completa (Coulomb, Gauss, potencial, capacitancia) + corriente y circuitos DC |
| `final` | 23.11.2026 | 07-11: magnetismo, inducción, inductancia y Maxwell |

## ⚠ Este temario es INFERIDO

El sílabo oficial **no está publicado** ("En proceso" en la web de la EPIM). Construido por inferencia por sándwich:

- **Pan de abajo, sólido:** el sílabo *real* de Física II (código 101060) tiene cinco unidades — Elasticidad, Oscilaciones, Ondas, Fluidos y Termodinámica. Su sumilla lo dice literal. **Ni Física I ni Física II tocan electricidad o magnetismo.**
- **Pan de arriba, débil:** Física III es prerrequisito de Circuitos Eléctricos I, y detrás vienen Máquinas Eléctricas, Electrónica y Control. Todos necesitan electromagnetismo. Pero sus sílabos tampoco están publicados.
- **Conclusión:** Física III = electricidad y magnetismo. No es conjetura por posición en la malla: es lo único del temario clásico de física para ingeniería que queda sin cubrir, y encaja con lo que exigen los cursos posteriores.

**Acción obligatoria: revisar en la semana 1 de clases (10-14 de agosto de 2026)** contra el sílabo real. Lo más probable que cambie: el corte parcial/final y cuánto entra de Maxwell (por eso el tema 11 está marcado `opcional: true`). Lo menos probable: que no sea electromagnetismo.

## Notas generales

**El electromagnetismo es invisible y esa es toda su dificultad.** No se resuelve con fórmulas sino decidiendo bien: qué superficie gaussiana, qué lazo de Ampère, qué sentido tiene la corriente inducida. Prioriza que el usuario dibuje antes de calcular — si no hay diagrama, no hay solución, y un problema de Gauss mal planteado no se salva con álgebra.

Las simulaciones de PhET valen aquí más que en cualquier otro curso del plan: ver un campo mueve la intuición más que veinte problemas resueltos a ciegas.

El punto de fricción previsible es `06-circuitos-dc-kirchhoff`, pero no por física: por resolver sistemas de tres ecuaciones sin equivocarse de signo. Si se atasca ahí, el problema está en `02-algebra`, no en este curso.
