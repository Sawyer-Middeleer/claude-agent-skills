---
name: organizing-notes
description: Keeps a markdown knowledge base consistent - frontmatter on every note, controlled tags, links between related notes and entities. Use whenever creating or editing notes in a knowledge base, and when asked to clean up, normalize, or reorganize notes. Reads the knowledge base's conventions.md as the authority.
---

# Organizing Notes

Three consistency passes — frontmatter, tags, links — applied whenever a note is touched. The knowledge base's `conventions.md` is the authority; read it first. If none exists, offer `/second-brain:setting-up-knowledge-base` before proceeding.

## Frontmatter

On every note touched (created, edited, moved — moves count):

1. **Frontmatter exists** — block starts `---` on line 1; add if missing
2. **`type:` matches the note's role** — per the conventions enum
3. **Keys are canonical** — migrate drifted forms (`date_created` → `created`, `last_updated` → `modified`, etc.)
4. **`modified:` bumped to today** — always

Rules: dates absolute `YYYY-MM-DD`; `tags:` always a YAML list; `snake_case` keys; don't invent metadata you can't derive.

## Tags

Tags are a controlled vocabulary — reuse before inventing.

1. Read the note's existing tags and the conventions' taxonomy
2. Apply matching namespaced tags (`area/...`, `status/...`)
3. Introduce a new tag only when ALL hold: no existing tag fits, the concept will recur across notes, and someone would actually query it
4. Never tag entities — `client/acme` is wrong; a link to the `acme` hub is right
5. Never tag what folder + title already say

## Links

Connections compound. When touching a note, look for missed link opportunities:

**Entity mentions** — first mention of any person, org, tool, or project that has a hub page gets linked (`[[Name]]` or `[Name](path)` per the conventions' link style). Don't over-link: once per note plus where prose demands it.

**Note-to-note** — when a note discusses a topic, decision, or event that lives in another note, link the other note. Search for candidates before assuming none exist.

**Hub creation** — an entity appearing in **≥3 notes** without a hub gets one: a short page (1–2 sentences of real content, not a bare stub) in the hubs folder, with the entity's obvious aliases. Then backfill links in the mentioning notes so the hub is immediately connected.

**Never link:** inside code blocks, URLs, or file paths; ambiguous names (two different "Davids" — skip rather than guess); generic capitalized words; inside existing links.

Bias conservative: a missed marginal link is cheaper than a wrong one.

## Batch cleanups

Asked to "clean up" or "normalize" the whole base:

1. Inventory first — report drifted frontmatter, orphan tags, unlinked entity mentions as counts before changing anything
2. Confirm scope with the user, then fix mechanically
3. Exclude anything the conventions or CLAUDE.md mark as off-limits
4. Summarize: files touched, by which pass, anything skipped and why
