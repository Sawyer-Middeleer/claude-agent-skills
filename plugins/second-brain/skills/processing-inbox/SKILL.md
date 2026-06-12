---
name: processing-inbox
description: Processes and routes files from the knowledge base's inbox folder into their proper homes. Use when asked to check, clean, process, sort, or organize the inbox. Categorizes by content analysis, moves what's clear, asks about what isn't, never deletes.
---

# Processing the Inbox

Clear the inbox by routing each file to its canonical home per the knowledge base's `conventions.md` folder map. Conservative bias: mis-filing costs more than asking.

## Workflow

### Step 1: Scan

List the inbox folder (`_inbox/` or whatever the conventions name). Empty → report "Inbox is empty" and stop.

### Step 2: Analyze Each File

- **Text** (`.md`, `.txt`): read the first ~2000 characters; judge from filename + content
- **Binary** (`.pdf`, `.docx`, images): judge from filename and extension only — don't parse content

For each file determine topic, type (note / reading source / daily capture / project doc / misc), destination per the folder map, and confidence. Files dated `YYYY-MM-DD` in the name are usually daily captures → the daily-notes structure.

### Step 3: Bucket

- **Clear** — confident destination, folder exists → move
- **Needs a folder** — confident destination, folder doesn't exist → propose it
- **Unclear** — low confidence → leave in place, ask

### Step 4: Execute and Report

Move all clear files (preserve filenames; on name collision propose a rename, never overwrite), then one consolidated report:

```
Inbox: 6 files processed

Filed (4):
- ai-agents-article.pdf → notes/reading/
- 2026-06-10-meeting.md → _daily/2026/06/

Proposed new folder (1):
- vendor-comparison.md → notes/procurement/ [NEW] — create and move?

Needs your call (1):
- random-thoughts.txt — couldn't categorize. Where should it go?
```

### Step 5: Resolve

Apply the user's answers; anything they defer stays in the inbox untouched.

## Rules

- **Never delete** — move or leave, only
- **One report**, not per-file interruptions
- **Don't edit content** during inbox processing — frontmatter and links are the organizing-notes skill's pass, after filing
- A file fitting two homes goes to the more specific one, noted in the report
- New-folder proposals follow the existing naming pattern of their siblings
