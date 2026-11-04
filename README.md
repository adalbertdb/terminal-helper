# tbtw

Pregúntale a Claude Haiku sobre lo que ves en tu terminal, estilo `/btw`. Pensado para Omarchy + [foot](https://codeberg.org/dnkl/foot).

`Ctrl+Shift+A` → popup flotante con el contexto de tu terminal ya cargado → escribes la duda → respuesta en ~2 s. Enter vacío cierra la sesión.

```
? como veo el rango de calidades de df?
Usa df["quality"].agg(["min", "max"])
```

## Cómo funciona

- foot tiene `pipe-scrollback`: manda el scrollback por stdin a un comando. Es la forma de "ver" la terminal sin plugins ni tmux.
- `tbtw` guarda las últimas 150 líneas y abre un `foot --app-id=tbtw` flotante.
- Cada pregunta va a `claude -p --model haiku --tools ""`: sin herramientas, sin sesión persistente, sin settings/hooks, ejecutado fuera de cualquier proyecto (no carga CLAUDE.md). Barato.

## Instalar

Requiere `claude` (logueado) y `foot`.

```bash
git clone https://github.com/adalbertdb/terminal-helper ~/proyectos/terminal-helper
```

`~/.config/foot/foot.ini`, bajo `[key-bindings]`:

```ini
pipe-scrollback=[/ruta/a/terminal-helper/tbtw] Control+Shift+a
```

`~/.config/hypr/hyprland.lua` (Hyprland con config Lua; Omarchy reciente):

```lua
o.window("tbtw", { float = true, size = "900 420", center = true })
```

Con config clásica: `windowrule = float on, match:class ^tbtw$` y `windowrule = size 900 420, match:class ^tbtw$`.

## Variables

| Variable | Default | |
|---|---|---|
| `TBTW_MODEL` | `haiku` | modelo de `claude --model` |
| `TBTW_LINES` | `150` | líneas de scrollback que se envían |

## Límites

- Solo foot. Otras terminales: kitty (`kitten @ get-text`), tmux (`capture-pane -p`) o ghostty necesitarían otro punto de entrada; el modo `--ask` es independiente de la terminal.
- Cada pregunta es independiente (no recuerda las anteriores).
- Una ventana `tbtw` a la vez.
