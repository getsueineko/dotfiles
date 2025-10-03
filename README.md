# For those too lazy to mess with Nix Package Manager 😅

Personal dotfiles managed with [chezmoi](https://chezmoi.io/).  
Keeps shell configs, app settings, and scripts consistent across machines.

![Demo](assets/demo.gif)

---

## Features & Overview

- **Portable**: one repo to sync configuration across all your machines  
- **Templated / data-driven**: support for machine-specific logic via `dotfiles` templating  
- **Script hooks**: scripts under `.chezmoiscripts` execute automatically during `chezmoi apply`  
- **Bootstrap logic**: run an initial setup via `dot_bootstrap`  
- **IDE support**: includes a list of VSCode extensions to keep editors in sync  
- Clean separation of config, data, and scripts  

---

## Structure & Directory Description

Here’s a brief description of key directories and files:

| Path | Purpose |
|---|---|
| `.chezmoidata/` | Data files (YAML, JSON, etc.) used by templates for conditional configuration |
| `.chezmoiscripts/` | Hook scripts (e.g. `run_*.sh`) that run when applying configuration |
| `dot_bootstrap/` | Scripts or logic for first-time machine setup |
| `dot_config/` | Configuration files (e.g. `~/.config/*`) maintained under version control |
| `dot_zshrc` | Your `~/.zshrc` or Zsh-related configuration |
| `vscode-extensions.txt` | List of VSCode extensions to install or sync |

> **Note:** chezmoi interprets names with `dot_` or similar conventions and creates appropriate symlinks into your home directory (or target paths).

---

## Prerequisites & Dependencies

Make sure you have installed:

- **git**
- **curl**
- **chezmoi**

---

## Installation

```bash
# Install chezmoi
sh -c "$(curl -fsLS https://chezmoi.io/get)"

# Initialize and apply configs
chezmoi init --apply https://github.com/$GITHUB_USERNAME/dotfiles.git
```

---

## Usage

- Update from repo: `chezmoi update`
- Apply changes: `chezmoi apply`
- Preview before applying: `chezmoi diff`

---

## TODO

- [x] ~~Add Nerd Font (**MesloLGS**) to ensure proper icons in terminals~~
- [x] ~~Add https://github.com/catppuccin/warp for Warp Terminals~~



