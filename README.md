# session-continuity

Pick up where you left off across Claude Code sessions — zero re-explanation.

## What it does

Every time a Claude Code session ends, `session-continuity` writes a snapshot of what you were working on, what decisions were made, and what's next. When a new session starts, Claude reads that snapshot automatically and picks up in context.

As of **v0.1.7**, snapshots are also saved before context compaction — so even long mid-session conversations don't lose state when Claude Code compresses the context window.

## Install

```bash
npm install -g session-continuity
```

## Setup (one-time per project)

```bash
cd your-project
sc init
```

This:
- Creates `.claude/session.md` (the rolling session store)
- Adds `@.claude/session.md` to `CLAUDE.md` so Claude reads it on startup
- Registers `Stop` and `PreCompact` hooks in `.claude/settings.json` so snapshots are written automatically

## Commands

| Command | Description |
|---|---|
| `sc init` | Wire up a project (run once) |
| `sc snapshot` | Write a snapshot now |
| `sc status` | Show current session state |
| `sc decide "<why>"` | Pin a permanent decision that survives the rolling window |
| `sc rotate` | Manual snapshot before force-quit |
| `sc clear` | Reset all session history |

## How snapshots work

Snapshots are written automatically — you don't need to do anything.

- **On session end** (`Stop` hook): captures git state + a narrative summary via `claude -p`
- **Before context compaction** (`PreCompact` hook, new in v0.1.7): captures state before Claude Code compresses the context window mid-session
- The last 3 sessions are kept in a rolling window in `.claude/session.md`
- Pinned decisions (via `sc decide`) persist permanently, outside the rolling window

## MCP server

`session-continuity` also ships an MCP server for cross-project context access:

```bash
sc-mcp
```

Add it to your Claude Code config to load session context from any project via the `load_session`, `save_session`, `pin_decision`, and `list_projects` tools.

## What gets committed

`.claude/session.md` is excluded from git by default (it's personal, machine-local state). `.claude/settings.json` (the hooks config) should be committed so your whole team gets the hooks.

## Feedback & Discussion

Something didn't work, felt missing, or you found a better workflow?

→ [GitHub Discussions](https://github.com/explorenav-dev/session-continuity/discussions)

## License

[PolyForm Noncommercial 1.0.0](LICENSE) — free for personal use.
