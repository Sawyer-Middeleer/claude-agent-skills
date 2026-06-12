---
name: capturing-knowledge
description: Proactively captures durable learnings into the knowledge base when conditions warrant - a session produced a reusable insight, a framework crystallized, a constraint was discovered, a decision trail completed. Triggers on "save this", "remember this", "worth keeping", "file that under", or the natural pause at the end of substantive work where a real learning emerged. NOT for process chatter, debugging logs, or tactical one-offs.
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

## Report

- Saved: one line — what and where, plus the tags/links applied
- Skipped: one line — what you considered and why it didn't clear the bar
