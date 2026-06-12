---
name: managing-daily-notes
description: Manages daily notes in a second-brain markdown knowledge base (a folder with a conventions.md or KB-declaring CLAUDE.md) - finding, creating, reading, and updating them, including task carryover with provenance. Trigger on "my daily note", "create today's daily note", "carry over my tasks", "close out the day" when working in such a knowledge base.
---

# Managing Daily Notes

Daily notes live at `_daily/YYYY/MM/YYYY-MM-DD.md` (or wherever the knowledge base's `conventions.md` says). Not every day has a note; gaps are normal.

## Finding

- **Today / a specific date:** search for the full path (`_daily/**/YYYY-MM-DD.md`) — never by bare filename, which can match elsewhere
- **Most recent:** highest year folder → highest month folder → highest date file
- Reading a daily note never needs permission — read freely

## Creating

1. Determine the target date; create the `YYYY/MM/` folders if missing
2. Structure: frontmatter (`type: daily`, `created`/`modified` = the date), a `## Tasks` section, a `## Notes` section — or the KB's own daily template if one exists in its templates folder
3. **Carry over tasks:** find the most recent prior note, copy every incomplete task (`- [ ]`) into the new note's Tasks section, preserving order. **Stamp each carried task with provenance** the first time it moves forward — append ` (since YYYY-MM-DD)` using the date of the note you're carrying from (preserve an existing `(since …)` rather than overwriting). This makes staleness computable later. **If more than ~15 tasks would carry over**, don't blindly copy them all — surface the count and ask the user whether to carry, drop, or defer the backlog; an unbounded carry compounds daily into noise.

## Updating

**Tasks:**

- Mark complete (`- [ ]` → `- [x]`) or reopen — no permission needed, just do it
- New tasks: tasks are ordered by priority top-to-bottom; ask where a new one ranks, or place it sensibly and say where
- Don't reorder existing tasks unless asked

**Notes section:** end-of-day summaries, work-in-progress context too verbose for a task line, and links to the notes touched today (per the KB's link style — daily notes are the spine the rest of the base hangs off).

## End-of-day pass (when asked to "close out the day")

1. Mark finished tasks; flag stale ones — using each task's `(since YYYY-MM-DD)` stamp, anything carried more than ~5 days — and ask whether to drop, defer, or keep them
2. Summarize the day's work in the Notes section, linking the relevant notes
3. Anything durable that surfaced today → offer `/second-brain:capturing-knowledge` rather than letting it die in the daily note
