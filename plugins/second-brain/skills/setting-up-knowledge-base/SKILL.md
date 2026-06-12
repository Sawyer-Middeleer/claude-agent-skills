---
name: setting-up-knowledge-base
description: Initializes a plain-markdown knowledge base Claude can maintain - folder structure, a conventions file, and a CLAUDE.md so every future session knows the rules. Use when the user wants to start a second brain, notes vault, or knowledge base, or wants Claude to take over maintaining an existing folder of notes. Works with Obsidian or any markdown editor.
---

# Setting Up a Knowledge Base

A knowledge base Claude can maintain needs three things: a predictable folder structure, written conventions, and a `CLAUDE.md` that loads those conventions into every session. This skill creates all three.

## Workflow

### Step 1: Scope

Use **AskUserQuestion**:

1. **Starting point** — new empty folder, or an existing folder of notes Claude should adopt?
2. **Primary use** (multiSelect) — work notes and decisions / research and reading / journal and daily logs / mixed personal+professional
3. **Editor** — Obsidian (or another `[[wikilink]]` app) vs plain markdown. Determines link style.

### Step 2a: New Knowledge Base

Create the minimal structure — sparse is fine, folders earn their existence:

```
<kb-root>/
├── CLAUDE.md             # from templates/kb-claude-md.md
├── conventions.md        # from templates/conventions.md
├── _inbox/               # unsorted captures land here
├── _daily/               # daily notes: _daily/YYYY/MM/YYYY-MM-DD.md
├── notes/                # the knowledge itself; topic subfolders as they earn existence
└── hubs/                 # entity pages (people, orgs, projects) — create when first needed
```

1. Copy both templates from `${CLAUDE_PLUGIN_ROOT}/skills/setting-up-knowledge-base/templates/` into the KB root (`conventions.md` and `CLAUDE.md`)
2. **Resolve every `<!-- SETUP: ... -->` marker** — in `conventions.md` § Links, delete the Internal line that doesn't match the editor chosen in Step 1, then delete the marker. Grep the copied files for `SETUP:` afterward; none should remain.
3. Fill the folder map to match what was actually created
4. Trim conventions the user won't use (e.g. drop the hubs section if they declined entity tracking)

### Step 2b: Existing Folder

Survey before touching anything, and never overwrite an existing authority.

1. **Map the structure** — list folders; sample 5–10 notes to read off the real patterns: where frontmatter sits and which keys recur, how files are named, how links are written (`[[wikilink]]` vs markdown), whether daily notes exist and where.
2. **Conventions authority:**
   - If a conventions document already exists (`conventions.md`, a `_meta/` doc, etc.), **make it the authority** — the sibling skills read it. Don't generate a competing one. Fill only genuine gaps, and only with the user's OK.
   - If none exists, write `conventions.md` codifying *what the survey found* — not the default template wholesale. Carry over the existing frontmatter keys, link style, and folder names verbatim so the skills match the vault instead of fighting it.
3. **CLAUDE.md:**
   - If one exists, **append a short "Knowledge base" pointer section** (read it first, merge — never clobber other project instructions).
   - If none exists, add one pointing at the conventions.
4. **Reorganization is a separate, opt-in step** — propose it, don't execute it. Adoption ≠ reorganization.

### Step 3: Hand Off

Confirm what was created, then point at the sibling skills that do the ongoing work:

- capture a learning → `/second-brain:capturing-knowledge`
- keep notes consistent → `/second-brain:organizing-notes`
- clear `_inbox/` → `/second-brain:processing-inbox`
- daily notes → `/second-brain:managing-daily-notes`

## Principles

- **Sparse is fine.** Empty folders and incomplete conventions beat elaborate unused structure.
- **The conventions file is the authority.** Every sibling skill reads it first; changes to conventions happen there, once, not in five skills.
- **When in doubt, leave it out.** A knowledge base stays useful by exclusion.
