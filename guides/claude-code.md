# Using Ix with Claude Code

Ix integrates with Claude Code through hooks that run automatically in the background.

## How It Works

### On Search (Grep/Glob)

When you or Claude searches for something, Ix runs a pre-search hook:

1. Claude runs `Grep "validateToken"`
2. Ix intercepts and runs `ix text "validateToken"` + `ix locate "validateToken"`
3. Graph-aware results are injected as additional context
4. Claude sees both the Ix results and the normal Grep output

This means Claude gets structural context (what calls it, what it's part of) before seeing raw text matches.

### On Edit (Write/Edit)

When a file is saved:

1. Claude edits `src/auth/provider.ts`
2. Ix auto-ingests the changed file in the background
3. The knowledge graph stays current without manual re-ingestion

## Installation

If Claude Code is installed when you run the Ix installer, the plugin is set up automatically. To install separately:

```sh
curl -fsSL https://raw.githubusercontent.com/ix-infrastructure/Ix/main/ix-plugin/install.sh | sh
```

## Requirements

- `ix` CLI in PATH
- Ix backend running (`ix docker start`)
- `jq` installed
- `rg` (ripgrep) installed

## Configuration

Hooks are registered in `~/.claude/settings.json`:

- **PreToolUse** hook on `Grep|Glob` — runs `ix-intercept.sh`
- **PostToolUse** hook on `Write|Edit|MultiEdit` — runs `ix-ingest.sh`

## CLAUDE.md Integration

Add Ix commands to your project's `CLAUDE.md` to guide Claude on how to use Ix:

```markdown
# Ix Memory

This project uses Ix Memory. Use `ix` CLI commands for codebase queries.

## Quick Reference
- `ix search <term>` — Find entities
- `ix explain <symbol>` — Understand a symbol
- `ix impact <target>` — Check blast radius before changes
- `ix overview <target>` — Structural summary
- `ix map ./src` — Re-map after significant changes
```

## Troubleshooting

**Hooks not firing?**
- Check `ix status` — backend must be running
- Check `which ix` — CLI must be in PATH
- Restart Claude Code after installing hooks

**Stale graph?**
- Run `ix map ./src` to re-ingest
- Or start `ix watch` for continuous updates
