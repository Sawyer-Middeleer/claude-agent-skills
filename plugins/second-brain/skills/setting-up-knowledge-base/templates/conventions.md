# Knowledge Base Conventions

Authority for how notes in this knowledge base are written, organized, tagged, and linked. Skills and humans both follow this file.

## What belongs

- **Durable** — still valuable in six months
- **Reusable** — informs future thinking, decisions, or work
- **Situated reasoning** — the "why" behind a decision that isn't in the deliverable
- **Entity anchors** — hub pages for people, organizations, tools, and projects referenced repeatedly

## What doesn't

- Process chatter, debugging logs, tool mechanics
- Information already canonical in another system (CRM, code, docs)
- Anything stale within 30 days
- Restatements of knowledge already here
- Generic summaries easily found anywhere

When in doubt, leave it out.

## Folder map

| Folder | Contents |
|---|---|
| `_inbox/` | Unsorted captures; cleared regularly by routing into the folders below |
| `_daily/YYYY/MM/` | Daily notes, `YYYY-MM-DD.md` |
| `notes/` | The knowledge itself; topic subfolders created when a topic has ≥3 notes |
| `hubs/` | One page per recurring entity (person, org, tool, project) |

## Frontmatter

Every note starts with:

```yaml
---
created:  YYYY-MM-DD
modified: YYYY-MM-DD   # bump on every edit
type:     note         # note | daily | hub | summary | synthesis
tags:     []
---
```

Dates absolute (`YYYY-MM-DD`), never relative. Keys in `snake_case`. Don't invent metadata you can't derive — leave it out.

## Tags

Controlled vocabulary — reuse before inventing. Prefer namespaced:

- `area/<domain>` — broad life/work areas (e.g. `area/work`, `area/health`)
- `status/{active,done,idea,archived}`

Introduce a new tag only when no existing tag fits AND the concept will recur AND you'd actually query it. Never tag entities — link to their hub instead.

## Links

<!-- LINK_STYLE: keep one of the two lines below -->
- Internal: `[[note-name]]` wikilinks; `[[note-name|display text]]` to control prose
- Internal: standard markdown `[display](relative/path.md)`

Link the first mention of any entity that has a hub. Create a hub when an entity appears in ≥3 notes. Don't link inside code blocks, URLs, or when the reference is ambiguous.

## One concept per file

A framework, a how-to, and a project decision are three notes, cross-linked — not one. Each concept has exactly one canonical home; everything else links to it.
