# session-continuity

Claude has no memory between sessions. You spend the first few minutes of every new session re-explaining the codebase, the branch, the decisions, and what comes next.

session-continuity fixes that — automatically.

```bash
claude mcp add session-continuity npx session-continuity
```

Open a project in Claude Code and start working. That's it.

---

## How it works

Claude calls four MCP tools — you do nothing.

| Tool | When | What it does |
|---|---|---|
| `load_session` | Session start | Restores context — status, decisions, next steps, branch |
| `save_session` | Session end | Captures an AI-generated snapshot of what happened |
| `pin_decision` | During session | Records key architectural choices that never expire |
| `list_projects` | On demand | Shows all projects with session history |

One-time setup. Claude handles the rest automatically, every session.

---

## What's captured

Every snapshot is an AI-generated briefing written to `.claude/session.md`:

```markdown
## Session — 2026-06-05 | feat/auth

Status: JWT refresh token flow — middleware done, route in progress.

Decisions made:
- httpOnly cookies (not localStorage) — prevents XSS
- 15-min access token TTL

Next steps:
- Wire /auth/refresh route
- Add token rotation on refresh

Blockers: None
```

- **Git state** — branch, status, recent commits
- **Status narrative** — one sentence on where things stand
- **Decisions + why** — key choices made this session with reasoning
- **Next steps** — ordered, most important first
- **Pinned decisions** — architectural choices that survive the rolling window

Up to 3 sessions are kept. Older sessions roll off. Pinned decisions never expire.

---

## CLI (setup and management)

```bash
sc status    # show what's saved for this project
sc rotate    # force a snapshot before a crash or force-quit
```

---

## Pairs with Waypoint

Session continuity captures *where you left off*. [Waypoint](https://github.com/explorenav-dev/waypoint-mcp) captures *where you are in the process* — across 14 guided development steps from first idea to ship.

Together: Claude knows both what was being worked on and where it sits in the dev journey. No manual briefing, ever.

```bash
claude mcp add waypoint npx @waycraft/waypoint-mcp
```

---

## Feedback & Discussion

Something didn't work, felt missing, or you found a better workflow?

→ [GitHub Discussions](https://github.com/explorenav-dev/session-continuity/discussions)

---

Free. Works with Claude Code. No configuration required.
