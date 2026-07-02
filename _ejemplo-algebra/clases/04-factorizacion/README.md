# Clase: Factorización

**Tema en temario:** `04-factorizacion`
**Fecha:** 2026-06-15
**Número de sesión para este tema:** 2

## Objetivo de esta sesión
Consolidar factorización de trinomios de la forma `x² + bx + c`, prestando atención al análisis de signos que en la sesión 1 se dejó flojo.

## Recuperación activa inicial
Se preguntó al usuario cómo factorizar `x² + 5x + 6` (visto en sesión anterior). Lo hizo bien: `(x+2)(x+3)`.
Luego se le pidió `x² - 5x + 6`. Respondió `(x-2)(x+3)` — **error típico registrado en `dif-001`**. No detectó que ambos factores deben ser negativos cuando el término independiente es positivo pero el coeficiente lineal es negativo.

## Resumen / apuntes
**Análisis de signos para `x² + bx + c`:**

| Signo de `c` | Signo de `b` | Signo de los factores |
|---|---|---|
| + | + | ambos positivos: `(x+p)(x+q)` |
| + | − | ambos negativos: `(x−p)(x−q)` |
| − | cualquiera | signos opuestos: `(x+p)(x−q)` — el mayor lleva el signo de `b` |

**Regla mnemotécnica trabajada con el usuario:** "el signo de `c` decide si los factores son iguales u opuestos; el signo de `b` decide cuál domina."

**Verificación obligatoria:** después de proponer los factores, multiplicar y comparar con el original. Toma 10 segundos y evita el error 100%.

## Ejercicios trabajados
1. `x² - 5x + 6` → primer intento `(x-2)(x+3)` (mal). Segundo intento con la tabla: `(x-2)(x-3)`. Verificó multiplicando. Correcto.
2. `x² + 7x + 12` → `(x+3)(x+4)`. Directo.
3. `x² - x - 12` → dudó con los signos. Aplicó la tabla: `c<0` → signos opuestos, `b<0` → el mayor negativo. Resultado: `(x-4)(x+3)`. Correcto.
4. `x² - 8x + 15` → `(x-3)(x-5)`. Directo, sin ayuda.
5. `x² + 2x - 15` → `(x+5)(x-3)`. Correcto, verificó.

Se hicieron 5 ejercicios seguidos aplicando la tabla. El usuario internalizó la regla.

## Herramientas usadas
- Cuaderno físico.
- Symbolab para verificar el ejercicio 3 cuando el usuario dudó de sí mismo.

## Dudas / puntos flojos
- Ninguna nueva. La dificultad `dif-001` (confusión de signos en trinomios) mostró mejora significativa. Se mantiene marcada como `resuelto: false` porque falta ver que se sostenga en próximas sesiones sin la tabla a la vista.

## Feynman de cierre
Se le pidió explicar "cómo saber los signos al factorizar". Explicó con la tabla y la regla mnemotécnica sin apoyo. Bien.

## Próximos pasos
- Sesión 3 del tema: pasar a `ax² + bx + c` (con coeficiente distinto de 1). Verificar que la regla de signos se sostiene sin la tabla.
- Al final del tema: mezclar ejercicios de todos los casos (aprendizaje intercalado) para verificar transferencia.
