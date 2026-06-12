# dot-claude

Actually useful Claude Code plugins for **knowledge work** — research, notes, ops, and analysis — not hardcore software engineering. Four plugins, install only what you want.

## Install

Add the marketplace once:

```
/plugin marketplace add Sawyer-Middeleer/dot-claude
```

Then install any plugin (the `@dot-claude` suffix names the marketplace explicitly, so install works even if another marketplace ships a like-named plugin):

```
/plugin install compound@dot-claude
/plugin install safeguard@dot-claude
/plugin install second-brain@dot-claude
/plugin install deep-research@dot-claude
```

Run `/reload-plugins` after installing if the new skills don't appear immediately. Plugins install at user scope (available in every project) by default.

Plugin skills are invoked namespaced — `/compound:creating-skills`, `/second-brain:processing-inbox` — or Claude triggers them automatically when their conditions match.

## The plugins

### compound — Claude that gets better every week

The thesis: do a task once by hand, codify it the second time, and fix the instructions every time they fail. Three skills that close the loop:

| Skill | What it does |
|---|---|
| `correcting-mistakes` | After an error gets resolved, finds the instruction file that caused it and applies a succinct fix — so the same mistake never happens twice |
| `codifying-tasks` | Notices when you're doing something for the second time and offers to turn the proven procedure into a reusable skill |
| `creating-skills` | Authors high-quality skills to spec — guided scoping, current frontmatter options, progressive disclosure, and a trigger-verification pass |

Built-in [auto memory](https://code.claude.com/docs) captures terse facts automatically; compound is the deliberate other half — it repairs the *instruction at its source* (not an external fact store) with root-cause triage, and its `creating-skills` reference is verified against the current Claude Code frontmatter spec.

### safeguard — seatbelts for knowledge work

Permission rules are enforced by the harness — stronger than anything you put in a prompt.

| Skill | What it does |
|---|---|
| `securing-claude` | Short scoping dialog → hardened `settings.json` (deny rules for secrets, asks for risky actions) that you approve before it's written |
| `auditing-access` | Read-only report of what Claude can currently see and do: settings layers, effective rules, connected MCP servers, hooks, additionalDirectories, plugins, and sensitive files no deny rule covers |

Rules are the floor: `securing-claude` also points you to built-in `/fewer-permission-prompts` for usage-mined allow-lists, and to sandboxing/PreToolUse hooks for the isolation deny rules can't give.

### second-brain — a knowledge base Claude maintains

**Already have years of notes? Point it at them.** second-brain's wedge is adopting an existing vault *in place* — it reads your folder, codifies your actual conventions, and maintains them, without reorganizing anything. (It also sets up a new base from scratch.) Plain markdown; works with Obsidian or any editor. One setup skill, four maintenance skills:

| Skill | What it does |
|---|---|
| `setting-up-knowledge-base` | Adopts an existing notes folder (codifying its conventions, never clobbering its files) or creates a new one — a `conventions.md` (the authority all other skills read) plus a `CLAUDE.md` so every session knows the rules |
| `capturing-knowledge` | Proactively saves durable learnings to their canonical home — with an aggressive filter for what *doesn't* belong |
| `organizing-notes` | Consistency passes: frontmatter on every note, controlled tags, links between related notes and entities |
| `processing-inbox` | Routes unsorted captures to their homes; moves what's clear, asks about what isn't, never deletes |
| `managing-daily-notes` | Daily logs with task carryover and an end-of-day close-out |

Capture leans on Claude noticing the moment, by design. Obsidian power users should pair it with [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) for deep format support (Bases, Canvas, callouts); second-brain owns curation and conventions, not format coverage. Note the overlap with built-in auto memory — second-brain keeps substantive notes in your KB; let auto memory keep terse machine-local facts.

### deep-research — multi-source research, filed properly

| Component | What it does |
|---|---|
| `deep-research` skill | Clarify scope → plan angles → validate sources (paywall-aware) → spawn parallel analysts → reconcile their output → synthesize by theme, verify high-stakes claims against originals, and emit a real URL bibliography. Deliverables are files, not a chat reply — and if you have a second-brain knowledge base, they file themselves following its conventions |
| `research-analyst` agent | Haiku-powered analyst spawned one-per-source for parallel deep reading |
| `analyzing-source` skill | The analysis framework each analyst follows |

The differentiator vs. native research or chat-based deep-research: output that **files itself into your knowledge base** as reusable, citation-linked notes — pairs directly with second-brain.

## Philosophy

- **Modular** — small composable skills, one concept each, install only the plugins you want
- **Compounding** — mistakes become corrections, repeated tasks become skills, learnings land in a knowledge base
- **Settings over prompts** — anything that must hold is a rule the harness enforces
- **Files over chat** — work products land somewhere durable and findable

## Migrating from v2

v2 shipped a single `dot-claude` plugin; v3 splits it into the four plugins above. Third-party marketplaces don't auto-update, so refresh the catalog first, then swap:

```
/plugin marketplace update dot-claude
/plugin uninstall dot-claude@dot-claude
/plugin install compound@dot-claude        # plus any others you want
/reload-plugins
```

The `/deep-research` command is now `/deep-research:deep-research`, and `creating-skills` + `correcting-mistakes` live in `compound`.

## Releasing changes

Each plugin's `version` lives in its `plugin.json` only (the single source of truth — the marketplace entry intentionally omits it to avoid a split-brain pin). When you ship a change to a plugin, bump its `plugin.json` version (semver) so installed users pick it up. CI runs `claude plugin validate --strict` on the marketplace and every plugin.

## License

MIT
