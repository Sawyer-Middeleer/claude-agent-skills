---
name: codifying-tasks
description: Turns recurring work into reusable skills. Use when a task being done now has been done before in this project, when the user says "make this repeatable", "turn this into a skill", "we'll be doing this again", or at the natural end of a multi-step task that will clearly recur. The rule of thumb - do it once by hand, codify it the second time.
---

# Codifying Tasks

Any task done twice will be done a third time. The second occurrence is the signal to codify it: the procedure is proven, the gotchas are known, and the cost of writing it down is lower than the cost of re-deriving it.

## When to fire

- The task you just completed resembles one done earlier in this project (similar prompt, similar steps, similar output)
- The user signals it: "make this repeatable", "turn this into a skill", "we'll do this again", "same as last time"
- A multi-step workflow just succeeded after trial and error — the corrected path is fresh and worth preserving

## When NOT to fire

- First occurrence of a task — wait for the second; premature codification bakes in guesses
- One-off work that won't recur (a migration, a single deliverable)
- The task is already covered by an existing skill — improve that skill instead (see the correcting-mistakes skill)
- The "procedure" is a single prompt with no steps, decisions, or gotchas — a skill adds nothing over just asking again
- The procedure carries **secrets or one-account specifics** (API keys, tokens, internal hostnames, a particular customer's data) — either skip it, or codify only the generalized shape with those values as arguments; never bake a secret into a saved skill

## Workflow

### Step 1: Offer, Briefly

One sentence, not a ceremony: "This is the second time we've done X — want me to codify it as a skill so next time it's one command?" If the user declines, drop it without argument.

### Step 2: Extract the Procedure from the Session

Reconstruct what actually worked — not an idealized version:

- The steps in the order they actually succeeded
- Decisions made along the way and what drove them
- Gotchas hit and the working resolution (state the correct approach directly; don't narrate the failure)
- Validation used to confirm the output was right
- Inputs that varied between the two occurrences — these become the skill's arguments
- Any secrets or one-account specifics — redact these from the procedure now; they become arguments, never literals

### Step 3: Decide Scope

Pick where the skill lives before authoring, so the file is written in the right place:

- Task specific to this project → `.claude/skills/<name>/`
- Task that recurs across the user's projects → `~/.claude/skills/<name>/`

When unsure, default to the project; promotion to user scope is easy later.

### Step 4: Author via creating-skills

Invoke the **creating-skills** skill (`/compound:creating-skills`) with the extracted procedure as raw material **and the Step 3 scope/save-location**. It handles naming, frontmatter, structure, and quality validation. Keep the scoping dialog short — most answers (procedure, arguments, save location) are already known from the session.

### Step 5: Verify and Hand Off

1. Tell the user the invocation (`/<name>`) and what arguments it takes
2. **A brand-new skill may not appear until the session reloads** — if it doesn't show in the skills list on the next message, tell the user to restart the session (or run `/reload-plugins` for a plugin skill). Don't report failure on the strength of it not showing immediately.
3. Suggest running it on the next real occurrence rather than a synthetic test — and fixing anything it gets wrong via the correcting-mistakes skill

## Principles

- **Codify the proven path.** The skill records what worked, not what should have worked.
- **Gotchas become instructions.** "Use the v2 endpoint" — not "Warning: the v1 endpoint fails."
- **Modular over monolithic.** If the session contained two separable procedures, write two skills.
- **The loop closes.** correcting-mistakes maintains what codifying-tasks creates. Together they compound.
