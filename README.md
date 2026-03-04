# dotfiles

Backend configuration repository for the [Homebrew Environment Bootstrap](https://gist.github.com/bsakdol/25396b101a6162b8e25acbfb89fd1abf) script.

This repository contains Homebrew package definitions (Brewfiles) and OS-specific shell configurations (dotfiles) that are automatically downloaded and applied by the bootstrap script when provisioning macOS or Linux systems.

## Quick Start

To bootstrap a fresh machine, run the one-liner:

```bash
curl -fsSL -o /tmp/brew-restore.sh https://gist.githubusercontent.com/bsakdol/25396b101a6162b8e25acbfb89fd1abf/raw/brew-restore.sh && bash /tmp/brew-restore.sh
```

The script will detect your OS, prompt you to select a profile, and configure your system using files from this repository.

## Overview

This repository is organized by:

- **Operating System** (macOS, Ubuntu/Linux)
- **Profile** (Developer, Minimal, Bastion)

Each profile includes:

- A **Brewfile** with curated package lists
- **Dotfiles** with shell and tool configurations specific to that profile

## Directory Structure

```text
dotfiles/
├── macos/
│   ├── profile-developer/    # macOS Zsh configs for full dev environment
│   │   ├── .zprofile
│   │   ├── .zshrc
│   │   └── .zsh_aliases
│   └── profile-minimal/       # macOS Zsh configs for minimal setup
│       ├── .zprofile
│       ├── .zshrc
│       └── .zsh_aliases
├── ubuntu/
│   ├── profile-developer/    # Ubuntu Bash configs for full dev environment
│   │   ├── .bashrc
│   │   ├── .bash_aliases
│   │   └── .bash_environment
│   └── profile-minimal/       # Ubuntu Bash configs for minimal setup
│       ├── .bashrc
│       └── .bash_aliases
├── universal/
│   ├── brewfiles/             # Package definitions (all OS-compatible)
│   │   ├── Brewfile-bastion
│   │   ├── Brewfile-developer
│   │   └── Brewfile-minimal
│   ├── .gitconfig             # Git configuration (Developer profile)
│   ├── .gitmessage            # Git commit message template (Developer profile)
│   └── .poshthemes/           # PowerShell themes
│       └── meridian-2.omp.json
└── README.md
```

## Profiles

### Developer

Complete engineering workstation environment including:

- DevOps and cloud tools (AWS, Kubernetes, Terraform, etc.)
- Programming language toolchains (Python, Node.js, Go, Rust, etc.)
- Security and networking utilities
- Development utilities and linters
- Git configuration with commit message templates
- PowerShell theme for enhanced terminal appearance

**Files:**

- `universal/brewfiles/Brewfile-developer`
- `macos/profile-developer/` (Zsh configuration)
- `ubuntu/profile-developer/` (Bash configuration)
- `universal/.gitconfig`, `.gitmessage`, `.poshthemes/`

### Minimal

Lightweight, essential-only setup for security-constrained or resource-limited systems:

- Essential utilities: `git`, `jq`, `yq`, `ripgrep`, `tmux`, `age`, `gnupg`
- No universal dotfiles or theme configurations
- Lean shell configurations

**Files:**

- `universal/brewfiles/Brewfile-minimal`
- `macos/profile-minimal/` (Zsh configuration)
- `ubuntu/profile-minimal/` (Bash configuration)

### Bastion

Specialized profile for hardened/security-focused systems.

**Files:**

- `universal/brewfiles/Brewfile-bastion`

## How It Works

The bootstrap script (`brew-restore.sh` in the Gist) performs these steps:

1. **OS Detection** - Determines if running on macOS or Linux
2. **Homebrew Installation** - Installs Homebrew if not present
3. **Profile Selection** - Prompts user to choose Developer or Minimal
4. **Package Installation** - Downloads and executes the selected Brewfile from `universal/brewfiles/`
5. **Dotfiles Application** - Downloads and applies OS and profile-specific dotfiles
6. **Cleanup (Optional)** - Prompts to remove packages not in the selected profile

### Cross-Platform Compatibility

- **Homebrew casks** are macOS-only; on Linux they are silently ignored
- **All Brewfiles** are designed to be cross-platform (macOS and Linux)
- **Shell configurations** are OS-specific:
  - macOS uses Zsh (`.zprofile`, `.zshrc`, `.zsh_aliases`)
  - Linux uses Bash (`.bashrc`, `.bash_aliases`, `.bash_environment`)
- **Universal dotfiles** (git config, themes) are applied only for the Developer profile

## Maintenance

### Adding or Updating Packages

Edit the appropriate Brewfile in `universal/brewfiles/`:

- Add packages as `brew "package_name"` (formulae) or `cask "package_name"` (macOS applications)
- Update comments to explain why the package is included
- Test on both macOS and Linux to ensure compatibility

### Updating Shell Configurations

Edit dotfiles in the respective profile directories:

- **macOS:** `macos/profile-{developer,minimal}/.z*`
- **Linux:** `ubuntu/profile-{developer,minimal}/.bash*`

Changes are applied the next time the bootstrap script is run.

### Adding New Profiles

To create a new profile (e.g., `profile-security`):

1. Create `universal/brewfiles/Brewfile-security`
2. Create `macos/profile-security/` with `.zprofile`, `.zshrc`, `.zsh_aliases`
3. Create `ubuntu/profile-security/` with `.bashrc`, `.bash_aliases`
4. Update the bootstrap script in the Gist to include the new profile option

## Notes

- Missing dotfiles in a profile are logged as informational notes but do not cause installation failures
- The bootstrap script is idempotent—running it multiple times is safe
- All Homebrew packages are pinned by version in Brewfiles for reproducibility
- PowerShell theme is only installed for the Developer profile on macOS

## Related Links

- **Bootstrap Script Gist:** [https://gist.github.com/bsakdol/25396b101a6162b8e25acbfb89fd1abf](https://gist.github.com/bsakdol/25396b101a6162b8e25acbfb89fd1abf)
- **Homebrew:** [https://brew.sh](https://brew.sh)
- **Oh My Posh (theme engine):** [https://ohmyposh.dev](https://ohmyposh.dev)
