# Installation

Ix requires Docker Desktop to run the backend (ArangoDB + Memory Layer).

## macOS / Linux

```sh
curl -fsSL https://raw.githubusercontent.com/ix-infrastructure/Ix/main/install.sh | sh
```

This installs:
1. The `ix` CLI to `/usr/local/bin`
2. Backend Docker containers (ArangoDB + Memory Layer)
3. Claude Code plugin (if Claude Code is installed)

## Homebrew

```sh
brew install ix-infrastructure/ix/ix
ix docker start
```

## Windows (PowerShell)

```powershell
irm https://raw.githubusercontent.com/ix-infrastructure/Ix/main/install.ps1 | iex
```

## From Source

```sh
git clone https://github.com/ix-infrastructure/Ix.git
cd Ix && ./setup.sh
```

## Verify Installation

```sh
ix --version
ix docker status
```

## Uninstall

```sh
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/ix-infrastructure/Ix/main/uninstall.sh | sh

# Homebrew
brew uninstall ix && brew untap ix-infrastructure/ix
ix docker stop --remove-data

# Windows
irm https://raw.githubusercontent.com/ix-infrastructure/Ix/main/uninstall.ps1 | iex
```
