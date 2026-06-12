---
name: correcting-mistakes
description: Self-corrects skill and instruction files after a genuine instruction defect is confirmed. Use when Claude errors during skill execution and finds the instruction was at fault, or when the user reports a mistake AND verification confirms the instruction (not the execution) caused it. Applies succinct fixes so a real defect never repeats — without mangling correct instructions on a false alarm.
---

# Correcting Mistakes

Trigger this skill when:

1. You error during execution of a skill or documented workflow and eventually work out a solution
2. The user indicates you made a mistake while following instructions
3. An edge case surfaces that the instructions don't account for

The goal: the same mistake should never happen twice — **without** corrupting correct instructions when the alarm is false. A confident-but-wrong user, or your own execution slip, must not drive a durable edit to a sound instruction. Diagnosis (Step 2) gates every change; many runs of this skill correctly end with no edit at all.

## Workflow

### Step 1: Locate the Instruction File

Find the file that produced the wrong behavior:

- Project skills: `.claude/skills/{skill-name}/SKILL.md` or its reference files
- User skills: `~/.claude/skills/{skill-name}/SKILL.md`
- Project instructions: `CLAUDE.md` (or a file it references)
- Plugin skills: installed plugin directories (see Step 1a)

Read the file to understand the current instructions.

### Step 1a: Note Whether the File Is Durably Editable

Skills installed via a plugin live in a managed cache that is overwritten on every plugin update — edits there are lost. If the faulty instruction is in an installed plugin, note that now but **don't record anything yet** — diagnosis (Step 2) may conclude no change is warranted. You'll route the correction in Step 4. Files under the project or user `.claude/` directories can be edited in place.

### Step 2: Verify the Claim, Then Diagnose

First, **confirm a mistake actually occurred.** When the trigger was a user assertion (Trigger 2), do not take it on faith — re-read the instruction and what you actually did, and check the claim against reality (run the command, re-read the source, check the doc). Sycophantic agreement that mangles a correct instruction is the failure this skill exists to prevent.

Then classify into one of three outcomes:

**(a) No mistake — instruction and behavior were both correct** (stop here, change nothing):

- The user's claim doesn't hold up on verification
- The output was actually correct; the disagreement is preference or misunderstanding, not error

Tell the user what you checked and why no change is warranted. Do not edit to placate.

**(b) Your misunderstanding — instruction was fine, execution slipped** (stop here, no file change):

- You misread or misapplied clear instructions
- Conversation context led you astray, not the instruction

**(c) Instruction issue** (proceed to Step 3):

- The instruction was ambiguous, misleading, or incomplete
- An edge case surfaced that the skill doesn't account for
- The instruction specified an incorrect approach or omitted a critical step

### Step 3: Prove the Fix Before Writing It

Never codify an unverified correction.

- **If the fix is a script, command, or tool use:** run the corrected approach in the current session and confirm it works before proceeding.
- **If the fix asserts a fact** (an API field name, a file path, a default, a documented behavior): verify the fact at its source — run it, read the file, check the live docs — before writing it. An unverified "correction" is just a new, confident guess.

Only proceed to Step 4 after confirmation.

### Step 4: Route, Then Apply the Correction

**Where the correction lands** (decided now, after diagnosis confirmed it's warranted):

- **Editable file** (project or user `.claude/`): edit it in place.
- **Plugin-cache file** (flagged in Step 1a): the cache edit would be lost on update, so record the correction where it persists — a short note in the project's `CLAUDE.md` (e.g. "When running /plugin:skill, do X instead of Y") or user memory. If the fix would help everyone, suggest the user open an issue or PR on the plugin's repo.

Then edit with these principles:

**Write for a reader with no memory of the error:**

- State the correct approach directly
- Do not reference the mistake, the error, or what was wrong before
- Do not add "Note:" or "Important:" warnings about the pitfall
- Do not explain why this is correct (unless explanation is inherently useful)

**Be succinct:**

- Change only what's necessary
- Preserve the existing style and structure
- Avoid adding defensive caveats or extra context

**Example — good correction:**

```markdown
# Before (incorrect)
Use `synthesis.md` as the template.

# After (correct)
Use `./templates/research-synthesis.md` as the template.
```

**Example — bad correction (too verbose, references the error):**

```markdown
# After (bad)
Use `./templates/research-synthesis.md` as the template.
Note: The file is named research-synthesis.md, not synthesis.md.
```

### Step 5: Confirm

After applying the fix:

1. Re-read the corrected section to verify it reads naturally
2. Confirm the fix is self-contained (no orphaned references to removed content)
3. Inform the user what was corrected, in one line
