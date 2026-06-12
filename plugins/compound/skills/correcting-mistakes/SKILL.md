---
name: correcting-mistakes
description: Self-corrects skill and instruction files after resolving errors. Use when Claude errors during skill execution and finds a solution, when the user indicates a mistake was made, or when an edge case breaks a documented workflow. Finds the relevant instruction file and applies succinct fixes so the mistake never repeats.
---

# Correcting Mistakes

Trigger this skill when:

1. You error during execution of a skill or documented workflow and eventually work out a solution
2. The user indicates you made a mistake while following instructions
3. An edge case surfaces that the instructions don't account for

The goal: the same mistake should never happen twice. Mistakes that get codified into corrected instructions are how a Claude setup compounds.

## Workflow

### Step 1: Locate the Instruction File

Find the file that produced the wrong behavior:

- Project skills: `.claude/skills/{skill-name}/SKILL.md` or its reference files
- User skills: `~/.claude/skills/{skill-name}/SKILL.md`
- Project instructions: `CLAUDE.md` (or a file it references)
- Plugin skills: installed plugin directories (see Step 1a)

Read the file to understand the current instructions.

### Step 1a: Plugin Skills Are Not Durably Editable

Skills installed via a plugin live in a managed cache that is overwritten on every plugin update — edits there are lost. When the faulty instruction is in an installed plugin:

1. Record the correction where it persists: add a short note to the project's `CLAUDE.md` (e.g. "When running /plugin:skill, use X instead of Y") or to user memory
2. If the fix would benefit everyone, suggest the user file an issue or PR on the plugin's repository

Only edit instruction files in place when they live in the project or user `.claude/` directories.

### Step 2: Diagnose the Root Cause

Determine: **Was this your misunderstanding, or an instruction issue?**

**Your misunderstanding** (stop here, no changes needed):

- You misread or misapplied clear instructions
- The instruction was correct but you made an execution error
- Context from the conversation led you astray, not the instruction

**Instruction issue** (proceed to Step 3):

- The instruction was ambiguous, misleading, or incomplete
- An edge case was found that the skill doesn't account for
- The instruction specified an incorrect approach
- The instruction omitted a critical step or detail

### Step 3: Test Before Fixing (if applicable)

**If the correction involves a script, command, or tool use:**

1. Test the correct approach in the current session
2. Verify it works as expected
3. Only proceed to Step 4 after confirmation

This prevents codifying a "fix" that doesn't actually work.

### Step 4: Apply the Correction

Edit the instruction file with these principles:

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
