# .zshrc Documentation

Auto-generated reference for this dotfile. Managed via [chezmoi](https://www.chezmoi.io/) — sections wrapped in `{{- if eq .chezmoi.os "..." }}` are OS-conditional templates rendered at apply time.

## Requirements

| Tool | Purpose | Required? |
|---|---|---|
| [Homebrew](https://brew.sh) | Package manager (Linuxbrew on Linux, Homebrew on macOS) | Yes |
| [zinit](https://github.com/zdharma-continuum/zinit) | Zsh plugin manager (self-installs on first run) | Yes |
| [starship](https://starship.rs) | Shell prompt | Optional (guarded) |
| [carapace](https://github.com/carapace-sh/carapace-bin) | Multi-shell completion engine | Optional (guarded) |
| [fzf](https://github.com/junegunn/fzf) | Fuzzy finder, powers `Ctrl+R`/`Ctrl+T`/`Alt+C` and `fzf-tab` | Optional (guarded) |
| [zoxide](https://github.com/ajeetdsouza/zoxide) | Smarter `cd` (`z <query>`) | Optional (guarded) |
| [rgrc](https://github.com/dabreiro/rgrc) | Generic output colorizer | Required for the `# ===== Tools =====` block to be error-free |
| [eza](https://github.com/eza-community/eza) | Used in `fzf-tab` directory previews | Optional (only used interactively) |
| [yazi](https://github.com/sxyazi/yazi) | Terminal file manager, wrapped by the `y` function | Required for `y` |
| [kubecolor](https://github.com/kubecolor/kubecolor) | Colorized kubectl output | Required for the `k` alias |
| [sops](https://github.com/getsops/sops) + age | Secrets encryption, referenced by `$AGE_KEY_FILE` | Optional |
| ansible-vault | Referenced by `$ANSIBLE_VAULT_PASSWORD_FILE` | Optional |

## Section reference

### History
- 10,000 entries, stored in `~/.zsh_history`.
- Shared and deduplicated across all open sessions (`share_history`, `hist_ignore_all_dups`, `hist_save_no_dups`).
- Timestamps written immediately per command (`inc_append_history_time`), not only on shell exit.
- ⚠️ `history` (bare) only prints the last 16 entries — this is standard zsh `fc -l` behavior, not a bug. Use `history 1` for the full log.

### Shell Options
- `correct` — suggests fixes for mistyped commands.
- `auto_cd` — typing a bare path `cd`s into it.

### PATH
- Prepends `~/.local/bin`, `~/bin`, and the krew plugin dir (`$KREW_ROOT/bin`, defaulting to `~/.krew/bin`).
- macOS only: adds `~/.lmstudio/bin` if present.

### Homebrew
- Loads `brew shellenv` **before** Starship/Carapace/zoxide, since those may be installed via Homebrew.
- Linux: Linuxbrew at `/home/linuxbrew/.linuxbrew`, guarded with `-x` so a missing install doesn't error.
- macOS: Homebrew at `/opt/homebrew` — **not guarded**; will error if Homebrew isn't installed at that exact path.

### Real Linux TTY *(Linux only)*
- Sets font `ter-v16b` once per session when running in a real (non-graphical) TTY (`$TERM == linux`).
- Switches Starship config: `starship-tty.toml` in a real TTY (simpler prompt, since some glyphs/colors don't render well), `starship.toml` everywhere else.

### Starship
- Loaded after PATH/Homebrew so the binary is guaranteed to resolve.
- No-ops silently if `starship` isn't installed.

### Zinit
- Self-bootstraps: clones zinit into `~/.local/share/zinit/zinit.git` on first run if missing.
- Loads 4 required annexes (`as-monitor`, `bin-gem-node`, `patch-dl`, `rust`) in non-turbo, light mode — required for other annexes/features to work.

### Completion Plugins
- `zsh-users/zsh-completions` loaded **before** `compinit` so its completion definitions are picked up during the compdump build.

### Completion System
- `compinit` rebuilds `.zcompdump` at most once every 24h (`compinit -C` skips the check on subsequent runs the same day) — meaningfully faster shell startup than an unconditional `compinit`.

### Carapace
- Bridges (`CARAPACE_BRIDGES`) fall back to native zsh/fish/bash completions for any command carapace doesn't have a spec for.
- No-ops silently if `carapace` isn't installed.
- ⚠️ Do **not** filter the `source <(carapace _carapace zsh)` output (e.g. via `grep`). The `compdef` lines it emits are what bind specific commands to `_carapace_completer` — stripping them leaves the completer function defined but unused.

### fzf Key Bindings
- Prefers the built-in `fzf --zsh` integration (fzf ≥ 0.48); falls back to the legacy `shell/key-bindings.zsh` file for older installs.
- `Ctrl+R` is only bound if the `fzf-history-widget` widget actually exists after sourcing — prevents binding a dead widget.

### fzf-tab
- Loaded **after** `compinit` and the fzf block — loading earlier causes some completion widgets to not be intercepted correctly.
- `Tab` is explicitly rebound to `fzf-tab-complete`.

### Plugins
- `zsh-autosuggestions` and `zsh-sudo` load after `fzf-tab` (both define/consume ZLE widgets that should exist by then).
- **`fast-syntax-highlighting` is intentionally loaded at the very end of the file** (see last section) — not here — because it must come after every plugin/script that defines its own ZLE widgets, or it prints `unhandled ZLE widget` warnings.

### fzf-tab (styles)
- History completion preview: current session (`fc -l 1`) plus last 5000 lines of `~/.zsh_history`, most recent first.
- `cd` preview: directory listing via `eza`.
- `z` (zoxide) preview: directory listing via `ls`.

### Completion Options
- Loads `LS_COLORS` via `dircolors -b` (needed by the `list-colors` style below).
- Case-insensitive-ish matching (`m:{a-z}={A-Za-z}`).
- Completion list uses `$LS_COLORS`.
- `menu no` disables the old-style completion menu (letting `fzf-tab` take over selection).

### Aliases
- `wttr` — quick weather via `wttr.in`.
- `k` — `kubecolor` (colorized kubectl).
- macOS only: `mc` (Midnight Commander with UTF-8 + xterm mouse support), `mcz` (mc's zsh cd-on-exit wrapper).

### Tools
- `zoxide init zsh` — enables `z <query>` smart-cd.
- `rgrc --aliases` — aliases common commands (`ls`, `cat`, `dmesg`, etc.) to colorized wrappers.
- **`unalias kubectl` immediately after `rgrc`** — required fix: `rgrc`'s colorized `kubectl` wrapper corrupts the machine-readable output of `kubectl __complete`, which silently breaks Carapace's kubectl completions (empty list, no error). This line un-aliases `kubectl` back to the real binary right after `rgrc` creates the alias.

### System Exports
- `$AGE_KEY_FILE` — age private key path, consumed by `sops`.
- `$ANSIBLE_VAULT_PASSWORD_FILE` — vault password file, consumed by `ansible-vault`/`ansible-playbook`.

### Yazi
- `y` — wraps the `yazi` file manager so that exiting it `cd`s the shell into whatever directory yazi was last in, via a temp cwd-file.

### Zinit Update
- `zinit-update` — runs `zinit self-update && zinit update --all`.

### Homebrew Upgrade
- `brew-smart-upgrade` — upgrades outdated Homebrew packages one at a time (instead of `brew upgrade` all-at-once), so one failing package doesn't abort the rest. Reports failed packages at the end.

### Syntax Highlighting
- `fast-syntax-highlighting`, loaded **last in the file on purpose**. It inspects all currently-defined ZLE widgets at load time; anything defined by a plugin or script that loads after it (e.g. `fzf --zsh`'s `fzf-history-widget`) won't be recognized, producing `unhandled ZLE widget` warnings on every shell start.

## Known gotchas (fixed in this config)

| Symptom | Cause | Fix applied |
|---|---|---|
| `unhandled ZLE widget 'fzf-history-widget'` on shell start | `fast-syntax-highlighting` loaded before `fzf --zsh` created the widget | Moved `fast-syntax-highlighting` to the end of the file |
| Carapace completions empty for every command | `source <(carapace _carapace zsh \| grep -v '^compdef ')` stripped the command→completer bindings | Removed the `grep` filter |
| `kubectl <Tab>` returns nothing via Carapace | `rgrc --aliases` aliases `kubectl` to a colorized wrapper, corrupting `kubectl __complete` output that Carapace parses | Added `unalias kubectl 2>/dev/null` right after the `rgrc` line |
| `zinit list` — `ERROR: Unknown subcommand` | `list` isn't a real zinit subcommand (it's an alias for `delete`/`uninstall`) | Use `zinit plugins` to list installed plugins instead |
| `history` (bare) shows only ~16 lines | zsh's `fc -l` default window, not a bug | Use `history 1` (or `fc -l 1`) for the full log |

## Verification checklist

See the accompanying checklist for a full manual walkthrough of every feature after changes to this file.