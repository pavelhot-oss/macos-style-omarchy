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