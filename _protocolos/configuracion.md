# Protocolo de configuración

> Comandos que el usuario puede invocar para activar/desactivar reglas de comportamiento del agente. La configuración persiste en `configuracion.json` en la raíz del proyecto.

---

## Referencia rápida de comandos

| Comando completo | Alias corto | Qué hace |
|---|---|---|
| `mostrar configuracion` | `cfg` | Lee `configuracion.json` y muestra al usuario el estado actual de cada opción con una breve descripción |
| `activar modo un tema por sesion` | `uts on` | Activa la regla "un tema por sesión" (pone `un_tema_por_sesion: true` en `configuracion.json`) |
| `desactivar modo un tema por sesion` | `uts off` | Desactiva la regla "un tema por sesión" (pone `un_tema_por_sesion: false` en `configuracion.json`) |

**Regla:** el agente acepta indistintamente la forma completa y el alias corto. Si el usuario dice algo parecido pero no idéntico a ninguno de los dos, pídele que use la forma exacta — no cambies configuración por conjetura.

**Convención de aliases:** ningún comando de este proyecto usa `/` (ver nota en el `README.md` raíz — evita choques con comandos propios de la herramienta que use el usuario, ej. Claude Code). En su lugar, cada comando tiene una forma completa en palabras y un alias corto intuitivo. `cfg` es el mnemónico raíz de este protocolo (de "configuración"). `uts` significa "un tema por sesión"; se combina con `on`/`off` para activar/desactivar. Cuando se agreguen nuevas reglas en el futuro, se les asigna un mnemónico corto (2-4 letras) documentado en la tabla, sin prefijo de barra.

---

## Cuándo aplicar este protocolo

Al inicio de cualquier sesión, el agente debe leer `configuracion.json` como parte del arranque (ver algoritmo en el `README.md` raíz). Si el archivo no existe, créalo con los valores por defecto (mira "Valores por defecto" abajo).

Cuando el usuario invoca uno de los comandos de la tabla, actualiza `configuracion.json` y confirma el cambio con una línea corta ("Modo 'un tema por sesión' activado.").

---

## Reglas configurables

### `un_tema_por_sesion` (booleano, default: `true`)

**Qué hace cuando está activo (`true`):**

En una misma sesión conversacional, el agente trabaja **exclusivamente un tema del temario**. Esto sirve para ahorrar tokens: cada sesión arranca con un contexto mínimo (solo el README, los JSON del curso, el tema actual) y no acumula historial de temas mezclados.

Comportamiento del agente cuando está activo:

1. **Al inicio de la sesión** (después del algoritmo de arranque), determina el tema único a trabajar:
   - Si hay un tema con `estado: "en_progreso"` en `progreso.json`, es ese.
   - Si no hay ninguno en progreso, es el próximo tema `"no_iniciado"` cuyos prerrequisitos internos estén completados.
   - Si hay varios candidatos y no está claro, pregunta al usuario cuál elige.

2. **Anuncia al usuario** al arrancar: "Modo 'un tema por sesión' activo. Vamos a trabajar solo `[tema_id]`. Si más tarde quieres pasar a otro tema, mejor cerramos aquí y abres una sesión nueva — así ahorras tokens al no acumular contexto."

3. **Si el usuario pide cambiar de tema mid-sesión**, no lo hagas directamente. Ofrécele dos opciones:
   - (a) Cerrar el tema actual (aplicar el protocolo de cierre normal, actualizar los JSON) y abrir una nueva sesión de Claude Code (u otro agente) para el próximo tema.
   - (b) Desactivar la regla ahora con `desactivar modo un tema por sesion` (alias `uts off`) y seguir en esta misma sesión.

4. **Al cerrar el tema** (ya sea completado o pausado), recuérdale explícitamente: "Cierro esta sesión aquí. Para el próximo tema, abre una nueva sesión de tu agente para ahorrar contexto."

**Qué hace cuando está inactivo (`false`):**

El agente trabaja libremente varios temas dentro de la misma sesión. Cambia de tema cuando el usuario lo pida, sin advertencias sobre tokens. Útil para:
- Agentes locales sin coste por token.
- Sesiones largas exploratorias donde el usuario quiere mezclar temas.
- Repasos que cruzan varios temas a la vez.

---

## Valores por defecto

Si `configuracion.json` no existe al inicio de una sesión, créalo con:

```json
{
  "un_tema_por_sesion": true,
  "notas": "Configuración del proyecto. Ver _protocolos/configuracion.md para los comandos que la modifican."
}
```

El default de `un_tema_por_sesion: true` favorece el uso económico para agentes de pago (Claude, GPT). Los usuarios con agentes locales pueden desactivarlo con un comando.

---

## Notas

- **La configuración es del proyecto, no del objetivo.** Persiste aunque el usuario complete objetivos y agregue nuevos (ver `_protocolos/expansion.md`).
- **No cambies la configuración sin comando explícito.** Si el usuario dice cosas como "estoy corto de tokens" o "hoy tengo tiempo para todo", NO alteres `configuracion.json` — coméntale que hay comandos disponibles y déjale la decisión.
- **Nuevos ajustes se agregan a este archivo** y a la tabla de comandos, con su propio protocolo de aplicación.
