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

1. Lee `objetivo.json` → objetivo actual, fecha límite, horas semanales.
2. Lee `configuracion.json` → estado de las reglas.
3. Para cada curso listado en `objetivo_actual.cursos_generados`:
   - Lee `temario.json` del curso → campo `modo_estudio`.
   - Lee `progreso.json` del curso.
   - Cuenta temas por estado (`no_iniciado`, `en_progreso`, `completado`).
   - Suma `sesiones_dedicadas`.
   - Calcula la **última sesión del curso** = el `ultima_sesion` más reciente entre sus temas (o "nunca" si todos son `null`).
   - Lee `dificultades.json` del curso y cuenta las que tienen `resuelto: false`.
4. Corre el **PASO 5.5 del `README.md` raíz** (selector de clase) para saber qué clase tocaría, pero **no la arranques**.
5. Calcula el **bloque de alertas** (ver abajo).
6. Presenta el resumen al usuario con este formato (adaptando cifras reales):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ESTADO DEL PROYECTO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Objetivo actual: [objetivo_final]
Deadline: [fecha_limite]  ·  [X] horas/semana  ·  faltan [N] semanas
Configuración: modo una-clase-por-sesión [ACTIVO / INACTIVO]

Cursos:
  [Nombre curso 1] — [ruta/cadencia/hito] — [N/M] temas — [S] sesiones
    └── Última sesión: [fecha o NUNCA]
    └── Tema en curso: [nombre del tema] ([porcentaje]%)
    └── Dificultades pendientes: [K]
  [Nombre curso 2] — ...

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
6. **Atraso de plan** — `sum(sesiones_dedicadas)` de un curso supera en >50% sus `sesiones_estimadas` acumuladas.
   `⚠ [Curso] va [N]% por encima de las sesiones estimadas.`

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
