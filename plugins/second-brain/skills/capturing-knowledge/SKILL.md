---
name: capturing-knowledge
description: Captures durable learnings into a second-brain knowledge base (a folder with a conventions.md or KB-declaring CLAUDE.md) when conditions warrant - a reusable insight, a framework crystallizing, a constraint discovered, a decision trail completed. Triggers on "save this to my notes/knowledge base", "file that under", "worth keeping", or the natural pause at the end of substantive work where a real learning emerged. Only operates when a knowledge base exists; not for process chatter, debugging logs, or tactical one-offs.
---

# Capturing Knowledge

Route a learning to its **canonical home** in the knowledge base and write it. The judgment of whether something IS a learning matters more than the speed of saving it.

## When to fire

Take initiative — don't wait for a command:

- A session concluded with a **reusable insight** — the kind future-you would want to find in six months
- The user signals it: "save this", "remember this", "that's worth keeping", "file that under X"
- A natural pause at the end of substantive work where a real learning surfaced
- A deliverable revealed a reusable lesson distinct from the deliverable itself

## Hard suppression — do NOT fire for

- Process chatter, debugging logs, tool mechanics
- Facts already canonical somewhere else (the CRM, the codebase, official docs)
- Anything stale within 30 days
- Restatements of what's already in the knowledge base
- Generic summaries and common knowledge findable anywhere
- Small tactical decisions that won't recur

When in doubt, leave it out. A run that saves nothing is fine.

## Step 0: Confirm a knowledge base exists

Before writing anything, locate the knowledge base: a `conventions.md`, or a `CLAUDE.md` that declares note conventions, at the working directory or an ancestor. **If none exists, do not write a note** — you are in an ordinary folder (often a code repo), not a knowledge base. Offer `/second-brain:setting-up-knowledge-base` and stop. This skill never scatters knowledge notes into arbitrary directories.

## The judgment bar

A saved learning must be both:

- **Findable** — it carries the tags and entity links that future search will use. If you can't predict what query would retrieve it, it isn't crystallized enough.
- **Usable** — it changes future work. "Fact noted" isn't a learning; "this changes how I'd approach X" is.

Fails either test → don't save; tell the user in one line what you considered and why you skipped it.

## Canonical routing

Read the knowledge base's `conventions.md` for the folder map, then route by what the learning IS:

| Kind of learning | Canonical home |
|---|---|
| Reusable mental model or decision framework | Its own note under `notes/` (or the KB's frameworks location) |
| Tool constraint, API gotcha, integration insight | A reference note for that tool — extend it if it exists |
| Insight about how a person or organization operates | That entity's hub page — add or extend a "How they operate" section |
| The "why" behind a project decision | That project's notes — a decisions note, appended chronologically |
| Research finding spanning sources | The relevant synthesis note |
| Reflection not durable enough for its own file | Today's daily note |

No fit after honest effort → a new note in the most specific existing folder. New folders almost never; state your reasoning when creating one.

## Authoring shape

New note or appended section, same form:

```markdown
## <Title — short, specific>

<2–4 sentences. The insight, stated cleanly. No padding.>

**Context.** What surfaced this — the session, decision, or data point.

**Implication.** What future work does differently because of it.
```

Apply the KB's frontmatter and link conventions (the organizing-notes skill covers both). Bump `modified:` when extending an existing note.

## Relation to built-in memory

Claude Code's built-in auto memory also responds to "remember this" / "save this," storing terse machine-local facts in `MEMORY.md`. That's a different store from this knowledge base, and splitting durable knowledge across both fragments it. Rule of thumb: a substantive learning (framework, decision trail, research finding) belongs **here**, as a real note in its canonical home; a one-line operating preference about how to work with this user can stay in built-in memory. When a capture has real content, prefer the knowledge base and say so.

## Report

- Saved: one line — what and where, plus the tags/links applied
- Skipped: one line — what you considered and why it didn't clear the bar
