---
name: auditing-access
description: Read-only audit of what Claude Code can currently see and do - settings layers, permission rules and modes, connected MCP servers, hooks, additionalDirectories, installed plugins, and sensitive files not covered by deny rules. Use when the user asks "what can you access", "audit my setup", "is anything exposed", or before starting work with confidential material. Makes no changes; never echoes secret or credential values.
---

# Auditing Claude Code Access

Produce a plain-English report of the current security posture. **This skill is read-only** — it changes nothing, and offers the securing-claude skill for remediation.

## Workflow

### Step 1: Enumerate Configuration

Read whichever exist (absence is a finding, not an error):

- `~/.claude/settings.json` — user defaults
- `.claude/settings.json` — shared project settings
- `.claude/settings.local.json` — personal project overrides
- Managed policy: `/Library/Application Support/ClaudeCode/` (macOS), `/etc/claude-code/` (Linux), `C:\Program Files\ClaudeCode\` (Windows)
- `.mcp.json` — project-shared MCP servers; note user-scope servers visible in `/mcp`. **Redaction:** MCP entries often carry API keys/tokens in their `env` blocks — note that a server is configured and what it connects to, but never echo credential values into the report or your reasoning.
- **Hooks** — `hooks` blocks in any settings file, and `hooks/hooks.json` in installed plugins: these run commands automatically and are part of the attack surface; list what fires on which event.
- **`additionalDirectories`** — extra paths granted beyond the project root; each widens what Claude can reach.
- **Installed plugins** — what's active (`/plugin`); each can ship skills, hooks, and MCP servers that carry their own access.

### Step 2: Resolve the Effective Permission State

1. Collect all `allow` / `ask` / `deny` rules across files — rules merge; deny wins everywhere
2. Note `defaultMode` and which file sets it
3. Flag risky grants: broad allows (`Bash(*)`, `WebFetch(*)`), `bypassPermissions` as default, allows that a stricter file contradicts

### Step 3: Scan for Uncovered Sensitive Files

Search the working tree (filenames only — do not open candidate files) for: `.env*`, `*.pem`, `*key*`, `credentials*`, `secrets/`, `*.p12`, `id_rsa*`, `token*`.

For each hit, check whether an existing deny rule covers its path. Uncovered hits are the headline findings.

### Step 4: Inventory Connected Surfaces

- MCP servers: name, scope (user/project), and what kind of access each implies (read? write? external service?)
- Note that every MCP tool runs with the server's own credentials — a connected CRM or email server is reachable regardless of file permissions

### Step 5: Report

```
## Access Audit — <project> — <date>

**Settings files found:** ...
**Default mode:** ... (set in ...)

**Protected (deny rules):** ...
**Asks before:** ...
**Runs without asking:** ...

**Exposed — sensitive files with no covering deny rule:**
- ./.env            ← not covered
- client-data/*.pem ← not covered

**Connected services (MCP):** ... (configured-only; no credentials shown)
**Hooks / additionalDirectories / plugins:** ...

**Recommendations:** (ranked, one line each)
```

End with one line: fixes available via `/safeguard:securing-claude`.

## Rules

- Never open or print the contents of suspected secrets, or credential values from MCP `env` blocks — paths and connection facts only
- Never edit anything, even obvious gaps — report and recommend
- If no settings files exist at all, state it accurately: there are no permission *rules*, so every decision falls back to the session's default mode — safe reads and read-only commands still run without prompting; risky actions prompt interactively. Nothing is protected by an enforced rule.
