# Permission Rules Reference

Current as of mid-2026. Authoritative source: https://code.claude.com/docs/en/permissions

## Where rules live

| File | Affects | Notes |
|---|---|---|
| `~/.claude/settings.json` | You, every project | Personal defaults |
| `.claude/settings.json` | Everyone on the project | Checked into git |
| `.claude/settings.local.json` | Just you, this project | Auto-gitignored when Claude Code creates it |
| Managed policy (IT-deployed) | Whole org | Cannot be overridden |

Rules **merge** across all files. Evaluation: **deny** → **ask** → **allow**. A deny in any file beats an allow in every file.

## Shape

```json
{
  "permissions": {
    "defaultMode": "acceptEdits",
    "allow": [ "Bash(npm run test:*)" ],
    "ask":   [ "Bash(git push:*)" ],
    "deny":  [ "Read(./.env)", "Read(./secrets/**)" ]
  }
}
```

## Rule syntax

- `Tool` or `Tool(*)` — every use of the tool
- `Bash(npm run build)` — exact command
- `Bash(npm run test:*)` — prefix match (`:*` suffix); `Bash(ls *)` glob form also works (space matters: `ls *` ≠ `ls*`)
- `Read(path)` / `Edit(path)` — gitignore-style patterns:
  - `~/...` — from home directory
  - `/src/...` — from project root
  - `//...` — absolute filesystem path (`//c/...` for C: on Windows)
  - `path` or `./path` — relative to current directory
- `WebFetch(domain:github.com)` — by domain
- `mcp__server` — every tool from an MCP server; `mcp__server__tool` — one tool

## Permission modes

| Mode | Behavior |
|---|---|
| `default` | Asks before anything risky; safe reads run |
| `plan` | Read-only; proposes before touching anything |
| `acceptEdits` | File edits auto-approved; commands still ask |
| `auto` | Most actions run with background safety checks |
| `bypassPermissions` | Never asks — disposable sandboxes only |

Cycle with `Shift+Tab` in the terminal; persist with `defaultMode`.

## Safe defaults (baseline for knowledge work)

```json
{
  "permissions": {
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(**/.env)",
      "Read(**/secrets/**)",
      "Read(**/*.pem)",
      "Read(**/credentials*)"
    ],
    "ask": [
      "Bash(git push:*)",
      "Bash(rm -rf:*)"
    ]
  }
}
```

Adjust per the user's actual sensitive paths — these patterns cover convention, not their reality.

## Limits to state honestly

- Deny rules govern Claude's tool calls. A subprocess (a Python script Claude runs) can still open files directly — use OS sandboxing for hard isolation.
- Symlinks: deny matches if either the link or its target matches; allow requires both.
- `/permissions` in-session shows every active rule and which file it came from.
