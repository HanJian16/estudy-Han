# Resistencia de Materiales (UNFV, IV ciclo)

> **Curso universitario real**, no de preparación. Código 28 del plan curricular 2019 de Ing. Mecatrónica, 3 créditos, 16 semanas. Sirve al objetivo `obj-2` (aprobar el 4to semestre).

## Prerrequisitos externos

> La verdad operativa está en los `prerrequisitos_externos` de cada tema del `temario.json`. Esto es el resumen para humanos.

De la malla UNFV: **Física II** (código 19) — y aquí sí es un requisito **de contenido**, no una compuerta. La Unidad I de Física II es *Elasticidad*: esfuerzo y deformación, módulo de Young, Ley de Hooke, Poisson, cizalladura, compresibilidad. Eso es literalmente el fundamento de este curso, y el usuario ya lo llevó.

De **Estática** (`11-estatica`), que se cursa **en paralelo**:

- `11-estatica/07-fuerzas-internas-cortante-momento` — sin diagrama de momento flector no hay flexión.
- `11-estatica/10-momentos-inercia` — el momento de inercia de la sección determina cuánto flexiona la viga.
- `11-estatica/04-equilibrio-cuerpo-rigido` — el método de las secciones es equilibrio aplicado a media pieza.

De los cursos de base de este proyecto:

- `05-fisica/06-estatica` — DCL y condiciones de equilibrio.
- `02-algebra/12-sistemas-ecuaciones` — la indeterminación estática *es* un sistema con ecuaciones de compatibilidad.
- `04-trigonometria/09-angulos-compuestos`, `04-trigonometria/10-angulos-multiples` — las ecuaciones de transformación de esfuerzo y el círculo de Mohr son ángulos dobles.

## ⚠ La secuencia con Estática es lo que hay que entender de este curso

Estática y Resistencia son **paralelos**: mismo ciclo, mismo requisito formal (Física II), ninguno depende del otro sobre el papel. Pero Resistencia **sí depende del contenido de Estática**.

La universidad lo resuelve secuenciando por dentro, y merece la pena verlo:

| | Semanas 1-8 (parcial) | Semanas 9-16 (final) |
|---|---|---|
| **Estática** | vectores → armaduras | **fuerzas internas**, **momentos de inercia** |
| **Resistencia** | esfuerzo, carga axial, torsión | **flexión** ← necesita lo de arriba |

Nada del parcial de Resistencia necesita Estática. La dependencia aparece en la semana 9, justo cuando Estática ya ha cubierto fuerzas internas. **Por eso funcionan en paralelo.**

**Consecuencia: si Estática se atrasa, Resistencia se bloquea en la semana 9** y con ella la mitad final del curso. El grafo de prerrequisitos ya lo arrastra solo, pero conviene saber por qué está así.

## Objetivo del curso

Aprobar el parcial (**28.09.2026**) y el final (**23.11.2026**). El listón: **bases suficientes para dar cara**, no dominio.

Es la base de Diseño de Máquinas y Elementos de Máquinas y Motores. Y es el primer curso donde se deja de calcular fuerzas para empezar a **dimensionar piezas reales**: ese cambio de mentalidad es lo que hay que llevarse.

## Reparto por hito

| Hito | Fecha | Temas |
|---|---|---|
| `parcial` | 28.09.2026 | 01-05: esfuerzo, deformación y propiedades, carga axial, indeterminación y térmico, torsión |
| `final` | 23.11.2026 | 06-11: flexión, cortante transversal, Mohr, cargas combinadas, deflexión, columnas |

## ⚠ Temario inferido — pero con el ancla más fuerte de los cuatro

El sílabo oficial **no está publicado**. Aun así, aquí el pan de abajo del sándwich es el mejor de todo el semestre: el sílabo *real* de Física II declara "Elasticidad" como Unidad I, con todo el aparato (Young, Hooke, Poisson, cizalladura, compresibilidad). **Resistencia arranca exactamente donde eso termina**, y eso no es una conjetura: es la razón de que Física II sea su requisito.

El pan de arriba sigue siendo débil (Diseño de Máquinas no tiene sílabo publicado), pero lo compensa que Resistencia es un curso muy estandarizado: Hibbeler, Beer & Johnston y Singer-Pytel lo cuentan casi igual.

**Revisar en la semana 1 de clases (10-14 de agosto de 2026).** Lo más probable que cambie: cuánto entra de deflexión de vigas y de columnas — en cursos de 3 créditos suelen recortarse, y por eso los temas 10 y 11 van `opcional: true`. Lo menos probable: que falten carga axial, torsión, flexión o Mohr.

## Notas generales

**El tema 02 solapa fuerte con Física II**, que el usuario ya llevó. Trátalo como recuperación activa, no como contenido nuevo: pregúntale primero qué recuerda del diagrama esfuerzo-deformación y ajusta. Si lo tiene fresco, se cierra en una sesión.

**El primer salto real es `04-carga-axial-indeterminacion`**: es donde la estática deja de bastar y hay que añadir geometría de la deformación. Es donde más gente se cae. No lo aceleres.

**Y `08-transformacion-esfuerzo-mohr` es el tema que más se memoriza y menos se entiende de toda la carrera.** Insiste en interrogación elaborativa: que explique qué representa un punto del círculo *antes* de dibujar ninguno. Reaparece en Diseño de Máquinas, así que memorizarlo aquí se paga caro después.
