---
name: securing-claude
description: Hardens Claude Code permissions with safe defaults for knowledge work. Use when the user wants to protect secrets or sensitive files, control what runs without asking, set up guardrails before working with confidential material, or says "secure my setup", "lock this down", "set up permissions". Writes settings.json rules the harness enforces - stronger than any prompt instruction.
---

# Securing Claude Code

Permission rules in settings files are enforced by the harness itself — they hold regardless of what any prompt says. This skill turns a short scoping dialog into a hardened `settings.json` the user approves before it's written.

Rule syntax, evaluation order, and modes: [reference/permission-rules.md](./reference/permission-rules.md). Read it before composing rules.

## Workflow

### Step 1: Scope the Posture

Use **AskUserQuestion**:

1. **What needs protecting?** (multiSelect)
   - Secrets and credentials (`.env`, keys, tokens)
   - Specific folders (client data, financials, personal) — ask which
   - Nothing sensitive here — just want sane defaults

2. **Risk posture** (single select)
   - Cautious — Claude asks before most actions
   - Balanced — safe operations run freely; risky ones ask (recommended)
   - Hands-off — most things run; only hard limits enforced

3. **Scope** (single select)
   - This project only (`.claude/settings.json`)
   - Just me, this project (`.claude/settings.local.json`)
   - All my projects (`~/.claude/settings.json`)

### Step 2: Read Current State

1. Read whichever of `~/.claude/settings.json`, `.claude/settings.json`, `.claude/settings.local.json` exist
2. Note existing rules — permission rules **merge** across files and a deny anywhere wins, so don't duplicate
3. **Record the strictest existing posture** — the strictest `defaultMode` and the existing deny/ask rules. The proposal must never come out weaker than this (see Step 4). In particular, a `defaultMode` written to `settings.local.json` overrides a stricter checked-in project mode, so never propose a looser local mode than the project already sets.
4. Scan the project for sensitive files the user may not have mentioned: `.env*`, `*.pem`, `*key*`, `credentials*`, `secrets/` — propose covering anything found

### Step 3: Compose the Rule Set

Build from the baseline in [reference/permission-rules.md](./reference/permission-rules.md) (§ Safe defaults), adjusted by the dialog:

- **deny** — secrets patterns always; user-named sensitive folders
- **ask** — outward-facing and destructive actions per posture (e.g. `Bash(git push:*)`)
- **allow** — only what the user's actual workflow needs to run unprompted
- **defaultMode** — `default` for cautious and balanced (it already runs safe reads freely and only asks on risky actions); reserve `acceptEdits` for hands-off **and** only in a backed-up or throwaway working directory, since it auto-approves destructive filesystem commands (`rm`, `mv`, `sed`), not just edits (see [reference/permission-rules.md](./reference/permission-rules.md) § Permission modes)
- **Path anchoring by scope** — when writing to user scope (`~/.claude/settings.json`), a bare `./.env`-style rule is interpreted relative to whatever project is open, which is rarely what's intended. Use the absolute `//`-anchored form there (e.g. `Read(//**/.env*)`); reserve project-relative paths for project settings files.

### Step 4: Present, Then Write

1. **Strictness check** — compare the proposal against the posture recorded in Step 2.3. If any part is weaker (looser mode, a dropped deny, a broader allow), keep the stricter existing rule and tell the user which and why. Never weaken silently.
2. Show the proposed JSON and explain each rule in one line of plain English
3. On approval, write to the file chosen in Step 1 — merge into existing content, never clobber other keys
4. Remind: changes apply on save; a deny here wins over any allow anywhere

### Step 5: Verify

1. Suggest `/permissions` to view the active rules and their sources
2. Test one deny with a **planted canary**, not a real secret: create a throwaway file at a denied path (e.g. `touch ./secrets/_canary` if `secrets/` is denied), attempt to read it, confirm the harness blocks it, then delete it. Don't exercise the deny against an actual secret — a misconfigured rule would surface its contents.
3. State the residual limits honestly: deny rules govern Claude's own tool calls; they don't bind arbitrary subprocesses or other programs. For hard isolation, enable sandboxing in settings or run in a container (see [reference/permission-rules.md](./reference/permission-rules.md)).

### Step 6: Offer the Stronger Tools (when warranted)

Permission rules are the baseline, not the ceiling. Surface these when they fit:

- **Building an allow-list from real usage** — don't hand-curate broad allows. Point the user at the built-in `/fewer-permission-prompts`, which mines the transcript for safe, repeated calls and proposes a tighter allow-list than guesswork.
- **Reliable destructive-command blocking** — if pattern-based ask rules aren't enough, offer an optional PreToolUse hook (see [reference/permission-rules.md](./reference/permission-rules.md) § Hooks vs. Bash rules).
- **Hard isolation** — for sensitive or untrusted work, recommend sandboxing/containers (same reference, § Sandboxing) rather than pretending deny rules contain subprocesses.

## Principles

- **Settings over prompts.** Anything that must hold belongs in a rule, not an instruction.
- **Deny narrowly, ask broadly.** Over-broad denies break workflows and get deleted; asks keep the user in the loop cheaply.
- **Never weaken silently.** If an existing rule is stricter than the proposal, keep the stricter rule and say so.
- **Rules are a floor.** For real isolation, reach for sandboxing and hooks — don't oversell what deny rules enforce.
