---
description: Omarchy UI tweaks (macOS-style binds, thumb buttons, window behavior). Loads the tweak memory and applies/edits/config-syncs.
---

# Omarchy UI tweaks — magic word

You are resuming work on **Omarchy UI tweaks** for this machine. Re-hydrate
context quickly, then act on the user's request.

## Context first (read these, in order)

1. Memory doc: this repo's `docs/macos-style-bindings.md`
   — the source of truth: full key map, file paths, revert/reuse notes.
2. Current live configs (assume `~HOME` for all):
   - `~/.config/hypr/bindings.lua` — Hyprland binds (macOS + thumb-button blocks)
   - `~/.config/foot/foot.ini` — terminal binds (`shell=/bin/zsh`, Alt+A scrollback)
   - `~/.zshrc` — zsh setup
3. Installer (for regenerating on new machines): this repo's
   `install-macos-style.sh`
4. Git repo to sync: `https://github.com/pavelhot-oss/macos-style-omarchy`
   (assume a local clone at `$HOME/macos-style-omarchy`; locate via the command
   body's own repo root if the clone lives elsewhere).

If `~/.config/hypr/bindings.lua` is absent, the destination machine has not been
set up yet — run `install-macos-style.sh` first (or the user may want to).

## Requested work

User opened with: **$ARGUMENTS**

Interpret and respond per these rules:

### Applying a tweak (the standard workflow)

1. **Back up** the file before editing: `cp file file.bak.<desc>.<ts>`.
2. **Edit only the marked blocks** inside `bindings.lua`
   (`macOS-style bindings -- BEGIN/END`, `Thumb-button tab switching -- BEGIN/END`)
   unless the user explicitly asks for something else. Keep the blocks
   self-contained so the installer script's heredoc stays in sync.
3. Validate:
   - `hyprctl reload` then `hyprctl configerrors` (must be clean).
   - `foot --config=~/.config/foot/foot.ini --check-config` (must be clean).
4. **Sync in sync:** after a confirmed working change, commit and push the same
   edit to the repo (the installer script's appended heredoc AND
   `docs/macos-style-bindings.md` table must match live config).
5. Tell the user to test before you push, unless they say otherwise.

### Reverting

Follow the `## Reverting` section of the memory doc exactly. Restore from the
`.bak.macos.*` backups if a block is mangled.

### Notes / gotchas to respect

- `o.bind`/`hl.dsp` are Omarchy's Hyprland Lua API. Never edit
  `/usr/share/omarchy/`.
- Plain `ALT+<letter>` is grabbed globally — GTK menu accelerators and some
  terminal Alt-combos are shadowed; this is intended.
- foot 1.28 has NO native `select-all`; Alt+A in terminals = pipe-scrollback to
  `wl-copy`.
- Multi-mod send uses comma-separated mods string (`"CTRL,SHIFT"`).
- `mouse:272/273/275/276` = LMB/RMB/BTN_SIDE/BTN_EXTRA.
- Changes here require no package installs; everything is config edits.