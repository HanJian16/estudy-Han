# Protocolo de utilidades

> Comandos rápidos de uso frecuente para consultar el estado del proyecto o hacer mantenimiento (backup). No cambian la configuración ni requieren flujos largos.

---

## Referencia rápida de comandos

| Comando completo | Alias corto | Qué hace |
|---|---|---|
| `mostrar estado` | `estado` | Muestra un resumen del progreso actual: objetivo, cursos, tema en curso, dificultades pendientes prioritarias |
| `generar backup` | `backup` | Genera un ZIP con el proyecto completo en `_backups/YYYY-MM-DD-HHMM.zip` |

**Regla:** el agente acepta indistintamente forma completa y alias. Si el usuario dice algo parecido pero no idéntico, pide la forma exacta.

---

## Comando `estado`

Lee y agrega información de varios archivos para dar una foto rápida del progreso. Se usa cuando el usuario abre el proyecto y quiere saber por dónde va sin arrancar una sesión de estudio.

### Pasos del agente

1. Lee `objetivo.json` → objetivo actual, fecha límite, horas semanales.
2. Lee `configuracion.json` → estado de las reglas.
3. Para cada curso listado en `objetivo_actual.cursos_generados`:
   - Lee `progreso.json` del curso.
   - Cuenta temas por estado (`no_iniciado`, `en_progreso`, `completado`).
   - Suma `sesiones_dedicadas`.
   - Lee `dificultades.json` del curso y cuenta las que tienen `resuelto: false`.
4. Presenta el resumen al usuario con este formato (adaptando cifras reales):

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ESTADO DEL PROYECTO
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Objetivo actual: [objetivo_final]
Deadline: [fecha_limite]  ·  [X] horas/semana
Configuración: modo un-tema-por-sesión [ACTIVO / INACTIVO]

Cursos:
  [Nombre curso 1] — [N/M] temas completados — [S] sesiones dedicadas
    └── Tema en curso: [nombre del tema] ([porcentaje]%)
    └── Dificultades pendientes: [K]
  [Nombre curso 2] — ...

Próxima sesión sugerida: [tema_id] — [nombre del tema]
  Razón: [en_progreso desde YYYY-MM-DD / siguiente elegible por prerrequisitos]

Handoff pendiente: [sí (tema X desde fecha) / no]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

5. **No arrancas ninguna clase después de esto.** Solo muestras el estado. Si el usuario quiere estudiar, invoca la sesión normal por separado.

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
