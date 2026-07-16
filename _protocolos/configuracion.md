# Protocolo de configuración

> Comandos que el usuario puede invocar para activar/desactivar reglas de comportamiento del agente. La configuración persiste en `configuracion.json` en la raíz del proyecto.

---

## Referencia rápida de comandos

| Comando completo | Alias corto | Qué hace |
|---|---|---|
| `mostrar configuracion` | `cfg` | Lee `configuracion.json` y muestra al usuario el estado actual de cada opción con una breve descripción |
| `activar modo una clase por sesion` | `ucs on` | Activa la regla "una clase por sesión" (pone `una_clase_por_sesion: true` en `configuracion.json`) |
| `desactivar modo una clase por sesion` | `ucs off` | Desactiva la regla "una clase por sesión" (pone `una_clase_por_sesion: false` en `configuracion.json`) |

**Regla:** el agente acepta indistintamente la forma completa y el alias corto. Si el usuario dice algo parecido pero no idéntico a ninguno de los dos, pídele que use la forma exacta — no cambies configuración por conjetura.

**Convención de aliases:** ningún comando de este proyecto usa `/` (ver nota en el `README.md` raíz — evita choques con comandos propios de la herramienta que use el usuario, ej. Claude Code). En su lugar, cada comando tiene una forma completa en palabras y un alias corto intuitivo. `cfg` es el mnemónico raíz de este protocolo (de "configuración"). `ucs` significa "una clase por sesión"; se combina con `on`/`off` para activar/desactivar. Cuando se agreguen nuevas reglas en el futuro, se les asigna un mnemónico corto (2-4 letras) documentado en la tabla, sin prefijo de barra.

> **Nota de renombre (julio 2026):** esta regla se llamaba `un_tema_por_sesion` (alias `uts`). El nombre viejo describía mal lo que hacía y chocaba de frente con el Método 3 (Aprendizaje Intercalado) del `README.md`, que pide mezclar ejercicios de varios temas en una misma sesión. La unidad correcta era la **clase**, no el tema: una clase puede tocar varios temas y sigue siendo una sola clase. Si encuentras `un_tema_por_sesion` en algún archivo o en un handoff viejo, es este mismo ajuste con el nombre anterior.

---

## Cuándo aplicar este protocolo

Al inicio de cualquier sesión, el agente debe leer `configuracion.json` como parte del arranque (ver algoritmo en el `README.md` raíz). Si el archivo no existe, créalo con los valores por defecto (mira "Valores por defecto" abajo).

Cuando el usuario invoca uno de los comandos de la tabla, actualiza `configuracion.json` y confirma el cambio con una línea corta ("Modo 'un tema por sesión' activado.").

---

## Reglas configurables

### `una_clase_por_sesion` (booleano, default: `true`)

**Qué hace cuando está activo (`true`):**

Una sesión conversacional contiene **exactamente una clase** (la unidad definida en "Anatomía de una clase" del `README.md` raíz). La sesión se cierra cuando la clase se cierra.

**Qué NO significa esta regla.** No es "solo se toca un tema". Una clase tiene como máximo **un tema nuevo**, pero alrededor puede tener repaso, ejercicios intercalados de temas ya cerrados y lo que la clase necesite — eso es la clase, no un desvío. Tampoco restringe qué archivos puede leer el agente: si para intercalar bien hace falta abrir el README de una clase vieja, se abre. Lo que la regla evita es que el chat se estire días acumulando historial, que es lo caro de verdad.

Comportamiento del agente cuando está activo:

1. **Al inicio de la sesión**, el tipo de clase ya viene decidido por el PASO 5.5 del algoritmo de arranque. Esta regla no lo decide; solo delimita hasta dónde llega la sesión.

2. **Anuncia al usuario** al arrancar: "Modo 'una clase por sesión' activo. La clase de hoy es `[tipo]`: `[tema_id o curso]`. Cuando la cerremos, mejor abrir una sesión nueva para la siguiente — así el chat no acumula contexto."

3. **Si el usuario pide abrir un SEGUNDO TEMA NUEVO a mitad de sesión**, no lo hagas directamente. Ofrécele dos opciones:
   - (a) Cerrar la clase actual (protocolo de cierre normal, actualizar los JSON) y abrir una sesión nueva para la siguiente clase.
   - (b) Desactivar la regla ahora con `desactivar modo una clase por sesion` (alias `ucs off`) y seguir en esta misma sesión.

   Ojo: esto aplica **solo a un segundo tema nuevo**. Si lo que pide es repasar algo viejo, hacer más ejercicios intercalados o revisar un prerrequisito, hazlo sin preguntar — eso es parte de la clase.

4. **Al cerrar la clase** (completada o pausada), recuérdale explícitamente: "Cierro esta sesión aquí. Para la próxima clase, abre una sesión nueva de tu agente para no acumular contexto."

**Qué hace cuando está inactivo (`false`):**

El agente encadena varias clases dentro de la misma sesión, sin advertencias. Útil para:
- Agentes locales sin coste por token.
- Sesiones largas exploratorias donde el usuario quiere abrir varios temas nuevos seguidos.
- Maratones de repaso que cruzan muchos cursos a la vez.

---

## Valores por defecto

Si `configuracion.json` no existe al inicio de una sesión, créalo con:

```json
{
  "una_clase_por_sesion": true,
  "notas": "Configuración del proyecto. Ver _protocolos/configuracion.md para los comandos que la modifican."
}
```

El default de `una_clase_por_sesion: true` favorece el uso económico para agentes de pago (Claude, GPT). Los usuarios con agentes locales pueden desactivarlo con un comando.

---

## Notas

- **La configuración es del proyecto, no del objetivo.** Persiste aunque el usuario complete objetivos y agregue nuevos (ver `_protocolos/expansion.md`).
- **No cambies la configuración sin comando explícito.** Si el usuario dice cosas como "estoy corto de tokens" o "hoy tengo tiempo para todo", NO alteres `configuracion.json` — coméntale que hay comandos disponibles y déjale la decisión.
- **Nuevos ajustes se agregan a este archivo** y a la tabla de comandos, con su propio protocolo de aplicación.
