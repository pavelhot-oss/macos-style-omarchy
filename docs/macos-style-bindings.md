# macOS-style bindings (ALT = Cmd) on Omarchy

Applied 2026-09-12. Turns the PC **ALT** key into a macOS-style **Cmd** for common
shortcuts, mirroring the existing SUPER-based Omarchy keybindings.

## Files changed

| File | Change |
|------|--------|
| `~/.config/hypr/bindings.lua` | Appended `macOS-style bindings -- BEGIN/END` block (10 binds) |
| `~/.config/foot/foot.ini` | Added `pipe-scrollback=[sh -c "wl-copy"] Mod1+a` under `[key-bindings]` |

Backups: `~/.config/hypr/bindings.lua.bak.macos.<ts>` and `~/.config/foot/foot.ini.bak.macos.<ts>`.

## The bindings

| Key | Action | What it does |
|-----|--------|--------------|
| `ALT+C` | Universal copy | GUI: sends Ctrl+C. Terminal: sends Ctrl+Insert |
| `ALT+V` | Universal paste | GUI: sends Ctrl+V. Terminal: sends Shift+Insert |
| `ALT+X` | Universal cut | sends Ctrl+X |
| `ALT+A` | Select all | GUI: sends Ctrl+A. Terminal: copies entire scrollback to clipboard (`wl-copy`) |
| `ALT+Z` | Undo | sends Ctrl+Z |
| `ALT+Q` | Quit app | Terminal: closes window. GUI: sends Ctrl+Q |
| `ALT+F` | Full screen | Hyprland `window.fullscreen` (fullscreen mode) |
| `ALT+W` | Close window | Hyprland `window.close` |
| `ALT+SPACE` | Omarchy menu | Spotlight-style launcher (`omarchy-menu toggle`) |
| `ALT+LMB` (drag) | Move window | Hyprland `window.drag` — mirrors `SUPER+LMB` |
| `ALT+RMB` (drag) | Resize window | Hyprland `window.resize` — mirrors `SUPER+RMB` |

Already present by default (no change needed): `ALT+TAB` / `ALT+SHIFT+TAB` window cycling.

**Skipped:** `ALT+M` (minimize) — not exposed by this Hyprland version's Lua API.
**Terminal note:** foot 1.28 has no native `select-all` action, so `ALT+A` in a
terminal pipes the whole scrollback to `wl-copy` instead of selecting.

## Thumb-button tab switching (added 2026-09-12)

Right-thumb side buttons are rebound (they no longer do the default
Back/Forward in browsers):

| Mouse button | Hyprland code | Sends |
|--------------|---------------|-------|
| Thumb-back (BTN_SIDE) | `mouse:275` | `Ctrl+Tab` (next tab) |
| Thumb-forward (BTN_EXTRA) | `mouse:276` | `Ctrl+Shift+Tab` (previous tab) |

Reuses the same `send_shortcut_once` helper. Globally scoped, like the ALT binds.
Multiple modifiers use comma-separated `mods` (e.g. `"CTRL,SHIFT"`), per the
Hyprland Lua dispatcher convention.

## Reverting

1. Delete the `macOS-style bindings -- BEGIN` … `-- END` block **and** the
   `Thumb-button tab switching -- BEGIN` … `-- END` block from
   `~/.config/hypr/bindings.lua` (restore from the `.bak.macos.*` file if you
   prefer to overwrite).
2. Remove the `pipe-scrollback=... Mod1+a` line from `~/.config/foot/foot.ini`.
3. Reload: `hyprctl reload` then check `hyprctl configerrors`; foot picks the
   change up in new windows (or `omarchy restart terminal`).

To restore plain browser Back/Forward on the thumb buttons, simply delete the
thumb-button block (step 1, first block) — no other files involved.

## Reusing on another machine

Copy these two files (keep the same relative paths):

```
~/.config/hypr/bindings.lua
~/.config/foot/foot.ini
```

Then reload as above. Requires Omarchy with the Hyprland Lua config (`o.bind`,
`hl.dsp.send_key_state`, `hl.dsp.window.fullscreen/close`) and foot + `wl-clipboard`
(`wl-copy`). Works regardless of whether the destination user also uses the
zsh setup (see `~/.zshrc` separately).

## Trade-offs / gotchas

- `ALT+<letter>` is grabbed globally at the WM level, so GTK **menu accelerators**
  (ALT+F opens the File menu) and in-app ALT combos never reach apps; the synthetic
  keys are what apps receive instead.
- zsh word-movement (Alt+F = forward word) and similar terminal Alt combos are
  shadowed inside terminals, since the WM intercepts first.
- `ALT+Q` in a terminal closes the terminal window (foot has no quit-key convention).
- The `send_key_state` helpers are copies of Omarchy's module-local ones from
  `default/hypr/bindings/clipboard.lua` — keep them in sync if defaults ever change.