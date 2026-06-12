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
| `acceptEdits` | File edits **and common filesystem commands** (`mkdir`, `touch`, `rm`, `mv`, `cp`, `sed`; PowerShell `Set-Content`, `Remove-Item`, etc.) auto-approved on in-scope paths; other commands still ask |
| `dontAsk` | Only pre-approved rules run; nothing else prompts — for locked-down/unattended use |
| `auto` | Most actions run with background safety checks |
| `bypassPermissions` | Skips permission checks entirely (a few hard circuit-breakers like `rm -rf /` still stop it) — disposable sandboxes only |

`Shift+Tab` cycles `default → acceptEdits → plan` (and `auto`/`bypassPermissions` if enabled); persist a starting mode with `defaultMode`.

Because `acceptEdits` auto-approves destructive filesystem commands, prefer `default` for any posture where unattended deletes/moves would be costly — reserve `acceptEdits` for throwaway or fully-backed-up working directories.

## Safe defaults (baseline for knowledge work)

```json
{
  "permissions": {
    "deny": [
      "Read(**/.env*)",
      "Read(**/secrets/**)",
      "Read(**/*.pem)",
      "Read(**/credentials*)"
    ],
    "ask": [
      "Bash(git push:*)",
      "Bash(rm *)",
      "PowerShell(Remove-Item *)"
    ]
  }
}
```

Adjust per the user's actual sensitive paths — these patterns cover convention, not their reality. `**/` matches `.env`, `.env.local`, `apps/web/.env.production`, and other nested locations. When writing to **user-scope** settings (`~/.claude/settings.json`), anchor with the absolute form so rules aren't interpreted relative to one project — e.g. `Read(//**/.env*)`.

## Limits to state honestly

- Deny rules govern Claude's tool calls. A subprocess (a Python script Claude runs) can still open files directly — use sandboxing for hard isolation.
- Symlinks: deny matches if either the link or its target matches; allow requires both.
- `/permissions` in-session shows every active rule and which file it came from — treat it as ground truth for what's currently enforced.

## Sandboxing (for the subprocess gap)

Permission rules constrain Claude's own tools, not programs those tools launch. When that gap matters, sandboxing is the answer the rules can't provide:

- **Filesystem/network sandbox** — Claude Code can run tool calls inside a sandbox that confines filesystem writes and network egress at the OS level, so even a spawned subprocess is contained. Enable it in settings (`sandbox`) for cautious/balanced postures handling sensitive material.
- **Container/VM** — for untrusted work or `bypassPermissions`, run the whole session in a disposable container so the blast radius is the container, not the host.

Check the current sandboxing options at https://code.claude.com/docs/en/settings before recommending exact keys — this surface evolves.

## Hooks vs. Bash rules for destructive commands

`Bash(...)` patterns are matched syntactically and are easy to slip past — `Bash(rm *)` misses `rm -fr`, `rm -r -f`, a `rm` inside a compound command, and PowerShell entirely. For reliable blocking of destructive commands, a **PreToolUse hook** that inspects the command and exits non-zero (exit 2) to block is the ecosystem-standard, harder-to-evade approach. Offer it as an optional add-on when the user wants real destructive-command guarding rather than prompt-on-match; pattern-based ask rules remain a fine lightweight default.
