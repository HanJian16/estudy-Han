# Protocolo de utilidades

> Comandos rápidos de uso frecuente para consultar el estado del proyecto o hacer mantenimiento (backup). No cambian la configuración ni requieren flujos largos.

---

## Referencia rápida de comandos

| Comando completo | Alias corto | Qué hace |
|---|---|---|
| `mostrar estado` | `estado` | Muestra un resumen del progreso actual: objetivo, cursos, tema en curso, dificultades pendientes prioritarias |
| `generar backup` | `backup` | Genera un ZIP con el proyecto completo en `_backups/YYYY-MM-DD-HHMM.zip` |

**Regla:** el agente acepta indistintamente forma completa y alias. Si el usuario dice algo parecido pero no idéntico, pide la forma exacta. Ningún comando usa `/` (ver convención en el `README.md` raíz).

---

## Comando `estado`

Lee y agrega información de varios archivos para dar una foto rápida del progreso. Se usa cuando el usuario abre el proyecto y quiere saber por dónde va sin arrancar una sesión de estudio.

### Pasos del agente

1. Lee `objetivo.json` → `presupuesto_horas` (línea de tiempo de periodos) y **todos** los `objetivos` con su `estado`, `prioridad`, `hitos` y `cursos_requeridos` (con sus `nivel_requerido` por curso y los `overrides` por tema). Agrupa la salida por objetivo, ordenada por prioridad, y marca los `latente` y `completado` como tales.
1b. Detecta los **cursos compartidos**: los que aparecen en el `cursos_requeridos` de más de un objetivo activo. Merecen mención propia: son los que rinden doble.
2. Lee `configuracion.json` → estado de las reglas.
2b. Lee `historial.json` y calcula el **ritmo real**:
   - `horas_por_sesion` = promedio de `duracion_min`/60. Con <5 sesiones, no lo uses: di "aún sin medir" y usa 1.5 h declarándolo como suposición.
   - `minutos_por_repaso` = **también se mide, no se inventa.** Toma las sesiones con `tipo_clase == "repaso"` y calcula `promedio(duracion_min / nº de temas repasados en esa sesión)`. Con <3 sesiones de repaso, di "aún sin medir" y usa **12 min** declarándolo como suposición. Este número alimenta el `horas_mantener` de las viabilidades de abajo, así que si es supuesto, dilo ahí también.
   - `horas_por_semana_real` = suma de duraciones de las últimas 4 semanas / 4. Con <3 semanas de datos, di "aún sin medir".
   - `horas_por_semana_real` **por objetivo** (filtrando por `objetivo_id`): revela a qué se está yendo el tiempo de verdad, que puede no ser lo que dice la prioridad.
3. Para cada curso requerido por algún objetivo activo (si lo requieren varios, muéstralo una vez y marca cuáles):
   - Lee `temario.json` del curso → campo `modo_estudio`.
   - Lee `progreso.json` del curso.
   - Cuenta temas por estado de flujo (`no_iniciado`, `en_progreso`, `completado`).
   - **Cuenta la PROFUNDIDAD real vs la requerida**, que es lo que de verdad dice si el curso está listo: por cada objetivo activo que lo pide, cuántos de sus temas ya tienen `nivel_alcanzado >= nivel_requerido` (curso + `overrides`) y cuántos no. "Listo para el semestre" y "listo para la UNI" son cuentas distintas sobre el mismo curso: muéstralas por separado si lo piden varios objetivos.
   - Suma `sesiones_dedicadas`.
   - Calcula la **última sesión del curso** = el `ultima_sesion` más reciente entre sus temas (o "nunca" si todos son `null`).
   - Lee `dificultades.json` del curso y cuenta las que tienen `resuelto: false`.
3b. **Viabilidad y predicción de olvido.** Para cada objetivo activo, en orden de prioridad, y dentro de él cada hito no condicional por fecha, corre la **cola con prioridad** del README ("Los objetivos compiten en una cola con prioridad"): calcula `horas_aprender` (los saltos de nivel que faltan, partiendo del `nivel_alcanzado` real), `horas_mantener` (los repasos que la escalera dispara hasta el hito × `minutos_por_repaso`), descuenta lo ya comprometido por objetivos más prioritarios sin recobrar cursos ya pagados, y decide CABE / NO CABE. En la misma pasada, arma la **predicción de olvido**: por cada tema, si `retención(nivel_repaso proyectado)` es menor que los días desde su último repaso hasta la fecha del hito, ese tema llegará frío. Es un aviso obligatorio, no opcional (ver "La durabilidad" en el README).
4. Corre el **PASO 5.5 del `README.md` raíz** (selector de clase) para saber qué clase tocaría, pero **no la arranques**.
5. Calcula el **bloque de alertas** (ver abajo).
6. Presenta el resumen al usuario con este formato (adaptando cifras reales):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ESTADO DEL PROYECTO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Presupuesto (línea de tiempo, en cola por prioridad):
  [desde] → [hasta]: [N] h/semana — [nota]     ← marca con «AHORA» el periodo vigente
  ...
Ritmo real: [H] h/sesión (medido sobre [N] sesiones | AÚN SIN MEDIR, se asume 1.5)
            [W] h/semana reales vs [D] declaradas → [coherente / desviación del X%]
Configuración: modo una-clase-por-sesión [ACTIVO / INACTIVO]

PRÓXIMO HITO: [nombre] de [objetivo] — [fecha] — faltan [N] semanas

━━ [prioridad P] OBJETIVO [id]: [objetivo_final]   [ACTIVO / LATENTE / COMPLETADO]
    (si LATENTE) Despierta cuando: [condicion_activacion] → [aún no se cumple / ¡YA SE CUMPLE!]
    Hitos:
      · [nombre] — [fecha] — faltan [N] sem — necesita [X] h (aprender [Xa] + mantener [Xm]), quedan [Y] h → [CABE / NO CABE por Z h]
      · [nombre] — [fecha] — ... 
      · [nombre] — condicional, no se planifica
    (viabilidad calculada con [H] h/sesión [medido/SUPUESTO] y [R] min/repaso [medido/SUPUESTO],
     descontando [W] h ya reservadas por objetivos más prioritarios, y sin recobrar los cursos
     ya pagados por ellos)
    Llegarán FRÍOS a [hito]: [tema_id (nivel_repaso proyectado N ≈ retiene D días, y el hito
      cae a E días de su último repaso)], ... → o "ninguno: todos llegan vivos"

    Cursos:
      [Nombre curso 1] — [ruta/cadencia/hito] — [S] sesiones
        └── Profundidad: [X/M] temas ya en el nivel que este objetivo pide ([nivel_requerido])
        └── Última sesión: [fecha o NUNCA]
        └── Tema en curso: [nombre del tema] (nivel_alcanzado: [nivel o —])
        └── Dificultades pendientes: [K]
        └── (si aplica) COMPARTIDO con: [otros objetivos que lo requieren]

━━ [prioridad P] OBJETIVO [id]: ...
    ... (repite por cada objetivo, ordenados por prioridad)

Cursos compartidos ([N]): [curso → objetivos que lo piden], ...
  Estos rinden doble: se estudian una vez y avanzan varios objetivos.

Repasos vencidos ([N]): [tema_id (vencido hace X días)], ...
Próximos repasos: [tema_id (fecha)], ...

⚠ ALERTAS
  [ver reglas del bloque de alertas; si no hay ninguna, escribe "ninguna"]

Próxima clase sugerida: [tipo] — [tema_id o curso]
  Razón: [qué rama del PASO 5.5 ganó y por qué]

Handoff pendiente: [sí (tema X desde fecha) / no]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

7. **No arrancas ninguna clase después de esto.** Solo muestras el estado. Si el usuario quiere estudiar, invoca la sesión normal por separado.

### Bloque de alertas (obligatorio)

> Esta es la red de seguridad del proyecto y la razón principal por la que `estado` existe. Un curso que nunca se estudia **no se queja**: por eso `07-aptitud-academica` estuvo dos semanas en cero sin que nada lo notara, hasta que el usuario lo preguntó. La arquitectura ya no debería permitirlo, pero ninguna arquitectura se cree a sí misma — este bloque es lo que lo verifica desde fuera.

Emite una alerta por cada caso que se cumpla. **No las omitas por parecer alarmista, y no las inventes por parecer útil.**

1. **Curso de cadencia abandonado** — un curso `cadencia` vencido por más de **3×** su `cadencia_dias`.
   `⚠ [Curso] lleva [N] días sin tocarse (cadencia: cada [C] días).`
2. **Curso nunca tocado** — un curso con todos sus `ultima_sesion` en `null` y el proyecto con más de 2 semanas de vida.
   `⚠ [Curso] no se ha tocado NUNCA desde que se creó el plan.`
3. **Hito vencido** — un curso `hito` cuya `condicion` ya se cumple pero sigue sin tocarse.
   `⚠ [Curso] cumple su condición de arranque y sigue sin empezar.`
4. **Hito urgente activo** — un curso `hito` cuya `condicion_urgente` se cumple.
   `⚠ [Curso] está en condición URGENTE: debería mandar sobre el resto de clases.`
5. **Repasos desbordados** — más de 5 temas con `fecha_proximo_repaso` vencida.
   `⚠ [N] temas con repaso vencido. Se están acumulando más rápido de lo que se cierran.`
6. **Atraso de plan** — `sum(sesiones_dedicadas)` de un curso supera en >50% la suma de sus `sesiones_por_nivel` hasta los niveles requeridos por sus objetivos (el coste estimado de aprenderlo a ese nivel).
   `⚠ [Curso] va [N]% por encima de las sesiones estimadas.`
7. **Latente que debería estar despierto** — un objetivo `latente` cuya `condicion_activacion` ya se cumple.
   `⚠ [Objetivo] cumple su condición de activación y sigue latente. Debería estar compitiendo por horas.`
   Es el hermano del canario de cursos huérfanos: un objetivo dormido de más no se queja solo.
8. **Hito inviable** — algún hito no condicional cuya validación (regla 10, cola con prioridad) ya no cabe.
   `⚠ [Hito] de [Objetivo] no entra para [fecha]: faltan ~[N] horas.`
   Si el hito es de examen universitario, **no es negociable**: la opción no es mover la fecha, es recortar alcance o aflojar otro objetivo.
9. **Objetivo prioritario cediendo** — el objetivo más prioritario (el de `prioridad` MENOR: 1 = lo primero) va atrasado mientras uno menos prioritario va al día.
   `⚠ [Objetivo prioritario] va atrasado y [Objetivo secundario] al día. Considera aflojar la cadencia del secundario.`
10. **Horas prometidas que no ocurren** — con ≥3 semanas de historial, `horas_por_semana_real` difiere >30% de la suma declarada.
   `⚠ El plan asume [S] h/semana pero llevas [W] h/semana reales. Todas las proyecciones de deadline están calculadas sobre horas que no están ocurriendo.`
   Es la alerta que solo el historial puede dar. Un plan puede tener la aritmética perfecta y aun así estar validado contra un usuario imaginario.
11. **Ritmo sin medir** — `historial.json` tiene menos de 5 sesiones.
   `⚠ Ritmo real aún sin medir ([N] sesiones). Las viabilidades de arriba asumen 1.5 h/sesión — trátalas como estimaciones, no como hechos.`
12. **Sobre-especificación de curso (jalar un curso entero donde bastan temas)** — un objetivo lista en `cursos_requeridos` un curso C con `nivel_requerido: "base"` (= lo que el cierre ya daría por defecto) y, al mismo tiempo, los OTROS cursos de ese objetivo ya referencian temas de C por `prerrequisitos_externos`. Señal de que C está declarado de más: el cierre transitivo lo traería igual, y **solo los temas que hacen falta**, en vez del curso completo.
   `⚠ [Objetivo] requiere el curso [C] entero a nivel base, pero sus cursos propios ya dependen de [n]/[m] temas de [C] por prerrequisito. Probable sobre-especificación (regla 12 del checklist): considera quitar [C] de cursos_requeridos y dejar que el cierre traiga solo lo necesario.`
   **Excepción legítima, NO la marques:** cuando el objetivo TRATA de C y lo pide a un nivel MAYOR que el default del cierre (ej. la UNI pide toda la base a `avanzado`) — ahí el curso entero sí es lo que el objetivo quiere, no un arrastre. La distinción es el nivel: `base` = redundante con el cierre; `intermedio`/`avanzado` = el objetivo lo trata de verdad.
   Esta alerta existe porque **este proyecto ya cometió ese error dos veces** (el semestre listaba los 6 cursos base enteros); la regla de "jalar temas, no cursos" estaba escrita pero nada la verificaba. Esto la verifica.
13. **Tema que se resiste (profundidad a re-verificar)** — un tema con `veces_repasado` alto (≥4) pero `nivel_repaso` bajo (≤2): se ha repasado muchas veces y aun así no consolida.
   `⚠ [tema] repasado [N] veces y sigue en nivel_repaso [M]. No consolida: su nivel_alcanzado ([nivel]) probablemente ya no es real — re-verifícalo antes de darlo por sabido para cualquier objetivo.`
   El `nivel_alcanzado` NO baja solo (la profundidad no se des-verifica automáticamente), pero este cruce es la señal de que quizás debería: trátalo como candidato a una clase de `repaso` dedicada y a re-comprobar el criterio de dominio. Es justo lo que `nivel_repaso` por sí solo esconde, porque se resetea al fallar y pierde el historial.

**Sugiere correr `estado` cada 2-3 semanas.** Es barato, no arranca nada, y es el único momento en que el proyecto se audita a sí mismo.

---

## Comando `backup`

Genera un ZIP del proyecto completo (excepto `_backups/`) con nombre `YYYY-MM-DD-HHMM.zip` dentro de la carpeta `_backups/`.

### Cuándo sugerirlo proactivamente

- Antes de invocar el protocolo de expansion (agregar nuevo objetivo).
- Antes de eliminar un curso a pedido del usuario.
- Antes de procesar un handoff de VUELTA si el usuario lo pide.

En estos casos, ofrece hacer un backup — no lo ejecutes sin permiso.

### Pasos del agente

1. Verifica si tienes capacidad de ejecutar comandos de sistema:
   - **Sí (Claude Code y similares):** procede al paso 2.
   - **No (agente sin shell):** avisa al usuario e indícale que puede correr manualmente en PowerShell:
     ```
     Compress-Archive -Path * -DestinationPath "_backups\$(Get-Date -Format 'yyyy-MM-dd-HHmm').zip" -Exclude "_backups"
     ```
     O el equivalente en su sistema.

2. Crea `_backups/` si no existe.

3. Genera el nombre del archivo con timestamp: `YYYY-MM-DD-HHMM.zip`.

4. Ejecuta el compresor apropiado:
   - **Windows PowerShell:** `Compress-Archive -Path * -DestinationPath "_backups\<nombre>.zip" -Exclude "_backups"`
   - **Bash (Linux/Mac):** `zip -r "_backups/<nombre>.zip" . -x "_backups/*"`

5. Verifica que el ZIP se creó y reporta al usuario:
   ```
   Backup generado:
     Archivo: _backups/YYYY-MM-DD-HHMM.zip
     Tamaño: X KB
     Contenido: N archivos

   Recomendación: si usas git+GitHub, hacer commit y push también protege este estado
   con historial y remoto. El backup local es complemento, no reemplazo.
   ```

6. **Política de retención:** no borres backups antiguos automáticamente. Si el usuario ve que `_backups/` crece mucho, que él decida qué eliminar (o considera sugerirle cambiar a git).

---

## Notas sobre respaldo del proyecto

**Recomendación primaria:** usar git + GitHub (o cualquier remoto). Ventajas frente al ZIP:
- Historia completa de cambios (diff por commit).
- Restauración selectiva de archivos.
- Remoto gratuito y accesible desde cualquier PC.
- Colaboración si en el futuro varias personas usan el mismo proyecto.

Si el usuario no está familiarizado con git, este proyecto se puede inicializar como repositorio con `git init` y luego pushear a un repo privado en GitHub. Cada cierre de clase importante puede terminarse con un `git commit` para dejar snapshot de progreso.

**Cuándo usar `backup` (ZIP local):**
- Usuario no quiere usar git.
- Necesita snapshot instantáneo antes de una operación grande.
- No tiene internet en ese momento.

Los dos mecanismos pueden coexistir.
