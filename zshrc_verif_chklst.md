# .zshrc Verification Checklist

A checklist to sanity-check every feature configured in `.zshrc` after any change.

## History

- [ ] `history` / `!!` work
- [ ] A command typed in one terminal shows up in another terminal's `history` right away (`share_history`)
- [ ] Typing the same command twice in a row doesn't duplicate it in history (`hist_ignore_dups`)
- [ ] `wc -l ~/.zsh_history` grows over time

## Shell options

- [ ] A typo (`gti status`) triggers a correction prompt (`correct`)
- [ ] Typing a bare directory path changes into it without `cd` (`auto_cd`)

## PATH / Homebrew

- [ ] `echo $PATH` includes `~/.local/bin`, `~/bin`, `$KREW_ROOT/bin`, and the Homebrew bin dir
- [ ] `brew --version` runs without errors
- [ ] `echo $XDG_DATA_DIRS` includes `linuxbrew/share`

## TTY / Starship

- [ ] Regular terminal shows the colored prompt (`starship.toml`)
- [ ] A real TTY (`chvt` / Ctrl+Alt+F2) shows the minimal prompt (`starship-tty.toml`) with the `ter-v16b` font

## Zinit

- [ ] `zinit plugins` lists all installed plugins with no load errors
- [ ] `zinit-update` runs and updates plugins without fatal errors

## Completion (compinit)

- [ ] `.zcompdump` exists and only gets rebuilt once every 24h, not on every shell start

## Carapace

- [ ] `echo ${_comps[helm]}` returns `_carapace_completer`
- [ ] `helm <Tab>`, `age <Tab>`, `restic <Tab>`, `hurl <Tab>`, `syft <Tab>` all return non-empty completions
- [ ] `kubectl <Tab>` works (requires `unalias kubectl` after the `rgrc` aliases)
- [ ] `kubectl get pods <Tab>` resolves real pod names when a cluster is reachable

## fzf

- [ ] `Ctrl+R` opens interactive history search
- [ ] `Ctrl+T` opens interactive file picker
- [ ] `Alt+C` opens interactive `cd`

## fzf-tab

- [ ] `Tab` opens the fzf menu instead of the plain completion list
- [ ] `cd <Tab>` shows an `eza` preview
- [ ] `z <Tab>` (zoxide) shows an `ls` preview

## ZLE plugins

- [ ] Typing a previously used command shows a grayed-out suggestion (`zsh-autosuggestions`), accepted with the right arrow
- [ ] `zsh-sudo` — double-tapping `Esc` prepends `sudo ` to the current command

## Syntax highlighting

- [ ] Valid/invalid commands are highlighted correctly
- [ ] A fresh shell start shows **no** `unhandled ZLE widget` warning

## Aliases

- [ ] `wttr` shows the weather
- [ ] `k get pods` (alias for `kubecolor`) prints colorized kubectl output
- [ ] `kubectl <Tab>` still works despite the `k` alias existing

## Tools

- [ ] `z <partial-path>` (zoxide) jumps to the right directory
- [ ] `ls` / `cat` / `dmesg` etc. print colorized output via `rgrc`
- [ ] `echo ${_comps[kubectl]}` returns `_carapace_completer` — not empty, not overridden by the `rgrc` alias

## System exports

- [ ] `$AGE_KEY_FILE` and `$ANSIBLE_VAULT_PASSWORD_FILE` point to existing files
- [ ] `sops` / `ansible-vault` don't prompt for a key/password manually

## Functions

- [ ] `y` opens yazi and changes `$PWD` to the selected directory on exit
- [ ] `zinit-update` updates zinit and all plugins
- [ ] `brew-smart-upgrade` upgrades packages one by one and reports any failures at the end

## Overall

- [ ] `time zsh -i -c exit` — startup time is reasonable
- [ ] `source ~/.zshrc` produces no errors or warnings

---

## Known gotchas

- **`fast-syntax-highlighting` must load last** — after every other plugin and
