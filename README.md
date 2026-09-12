# macos-style-omarchy

Bring macOS keyboard muscle memory (and right-thumb mouse buttons) to an
Omarchy Linux box — Arch + Hyprland + foot.

## What it does

| Action | Detail |
|--------|--------|
| Install | `zsh`, `zsh-autosuggestions`, `wl-clipboard` |
| `~/.zshrc` | starship prompt, bash completion compat, history, autosuggestions |
| Login shell | `chsh -s /bin/zsh` |
| Hyprland binds | `ALT+C/V/X/A/Z/Q/F/W/SPACE` = Cmd-style, terminal-aware copy/paste |
| Thumb buttons | `mouse:275` → `Ctrl+Tab`, `mouse:276` → `Ctrl+Shift+Tab` |
| foot | `shell=/bin/zsh`, `Alt+A` copies whole scrollback via `wl-copy` |

See [`docs/macos-style-bindings.md`](docs/macos-style-bindings.md) for the
full key mapping, trade-offs, and manual revert/reuse instructions.

## Usage

```bash
./install-macos-style.sh            # apply everything
./install-macos-style.sh --dry-run  # preview without changes
./install-macos-style.sh --no-zsh   # skip zsh / shell switch
./install-macos-style.sh --no-hypr  # skip Hyprland bindings
./install-macos-style.sh --no-foot  # skip foot config
```

Idempotent and safe: re-running skips what is already applied, and every
modified file is backed up to `<file>.bak.macos.<timestamp>` first.

## Magic word for opencode (global `/omarchyUI`)

The repo ships a global slash-command at `.opencode/command/omarchyUI.md`. It
loads the tweak memory, finds the live configs, and applies/reverts/ syncs the
repo — a one-stop entry point.

To install it globally:

```bash
cp .opencode/command/omarchyUI.md ~/.config/opencode/command/
```

Then restart opencode. Use it like:

```
/omarchyUI make alt+f true fullscreen
/omarchyUI revert the thumb button block
```

The global copy works from any directory. The project-level version (inside
the cloned repo) also works when your cwd is the repo itself.