# ix upgrade

Upgrade ix CLI and backend to the latest version.

## Usage

```sh
ix upgrade
```

## What It Does

1. Checks GitHub for the latest release
2. Downloads the new CLI tarball for your platform
3. Pulls the latest Docker image
4. Restarts the backend if it's running

## Flags

| Flag | Description |
|------|-------------|
| `--check` | Only check for updates, don't install |

## Examples

```sh
# Check if there's an update
ix upgrade --check

# Upgrade everything
ix upgrade
```

## Auto-Notification

Every `ix` command checks for updates in the background (cached for 1 hour). If a new version is available, you'll see:

```
  Update available: 0.4.0 → 0.5.0
  Run: ix upgrade
```
