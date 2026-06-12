---
name: creating-skills
description: Creates high-quality Claude skills following official best practices. Use when the user asks to create a new skill, improve an existing skill, or needs guidance on skill authoring. Includes guided scoping, proper structure, naming conventions, progressive disclosure patterns, current frontmatter options, and quality validation.
---

# Creating High-Quality Claude Skills

Use this skill when creating or improving Claude skills to ensure they follow official best practices and work effectively across all Claude models.

## Core Principles

**Conciseness**: Skills share the context window. Only include information Claude doesn't already have.

**Appropriate Freedom**: Match instruction detail to task needs:

- **High freedom**: Multiple valid approaches (use text instructions)
- **Medium freedom**: Preferred patterns with flexibility (use pseudocode/guidance)
- **Low freedom**: Exact sequences required (use specific bash/python scripts)

**Progressive Disclosure**: Keep SKILL.md under 500 lines. Split content into reference files when approaching this limit. All references must be one level deep.

## Phase 1: Initial Clarification

**BEFORE planning or writing the skill**, use the **AskUserQuestion** tool to gather structured feedback on skill requirements — but only for questions the conversation hasn't already answered.

1. **Instruction detail level**: How much freedom should Claude have? (single select)
   - High freedom — multiple valid approaches, text instructions only
   - Medium freedom — preferred patterns with flexibility, pseudocode/guidance
   - Low freedom — exact sequences required, specific scripts

2. **Success criteria**: How will success be measured? Suggest options contextually based on skill type. (multiSelect)

3. **Invocation**: Who triggers this skill? (single select)
   - Claude, proactively when conditions match (default)
   - Only the user, via slash command (`disable-model-invocation: true`)
   - Only Claude, never shown in the menu (`user-invocable: false`)

4. **Save location**: where does the skill live? (single select; skip if the caller already specified it — e.g. codifying-tasks passes this in)
   - This project only → `.claude/skills/<name>/`
   - All my projects → `~/.claude/skills/<name>/`

Add 1–2 contextual questions only if essential to clarify workflow steps, inputs/outputs, or materials to include.

## Phase 2: Design & Planning

1. **Choose skill name**
   - [ ] Use gerund form (verb + -ing, max 64 chars)
   - [ ] Lowercase letters, numbers, hyphens only
   - [ ] Avoid: `helper`, `utils`, or words containing "anthropic"/"claude" — reserved

2. **Write description**
   - [ ] State what the skill does AND when to use it (trigger phrases)
   - [ ] Include specific keywords for discovery
   - [ ] Keep under 1024 chars, write in third person
   - [ ] Think: "How will this be found among 100+ skills?"

3. **Plan content structure**
   - [ ] Single file (< 400 lines) or progressive disclosure
   - [ ] Identify sections: workflow, examples, templates, validation
   - [ ] Note which parts need high/medium/low freedom

4. **Architecture diagram (optional)** — for a multi-file skill with reference docs, templates, or branching flows, an ASCII diagram per [reference/architecture-diagrams.md](./reference/architecture-diagrams.md) helps validate structure. Skip it for a simple single-file skill; don't spend tokens diagramming something trivial.

5. **Present plan to user**
   - Proposed name, description, structure outline (and the diagram if you drew one)
   - Adjust based on feedback before writing

## Phase 3: Create Content

Write into the save location chosen in Phase 1 (`<location>/skills/<name>/SKILL.md`).

1. **Write SKILL.md** — valid frontmatter, only non-obvious information, checklists for complex workflows
2. **Add supporting files** if needed — `templates/`, `reference/`, `scripts/`, one level deep, SKILL.md stays under 500 lines
3. **Implement feedback mechanisms** identified in Phase 1 — validation checkpoints, quality loops, error handling

A brand-new skill may not load until the session restarts (or `/reload-plugins` for a plugin skill); tell the user rather than treating a not-yet-listed skill as broken.

## Frontmatter Reference

**Required shape** (only `description` is strictly recommended; `name` defaults to the directory name):

```yaml
---
name: skill-name-here
description: What this does and when to use it, with trigger keywords.
---
```

**Optional fields worth knowing** (current as of mid-2026):

| Field | Use for |
|---|---|
| `argument-hint` | Autocomplete hint, e.g. `[filename] [format]` |
| `arguments` | Named positional args for `$name` substitution (`$ARGUMENTS` = all) |
| `disable-model-invocation: true` | User-only skills (side effects, cost) |
| `user-invocable: false` | Claude-only skills (hidden from `/` menu) |
| `allowed-tools` / `disallowed-tools` | Pre-grant or remove tools while the skill runs |
| `model`, `effort` | Override model or effort for this skill |
| `context: fork` + `agent:` | Run the skill in a forked subagent instead of the main loop |
| `paths` | Glob patterns; skill activates only when working with matching files |
| `when_to_use` | Extra trigger phrases appended to description (combined cap 1536 chars) |

Skills installed via a plugin are invoked namespaced: `/plugin-name:skill-name`. Design names that read well after the colon.

## Content Organization

**Single SKILL.md when:** under 400 lines, tightly related, needed all at once.

**Progressive disclosure when:** approaching 500 lines, distinct topics, conditional/advanced detail:

```
SKILL.md (overview + core workflow)
├── templates/        # files the skill fills in or copies
└── reference/        # detail loaded only when needed (one level deep, never nested)
```

## Content Principles

**Challenge every detail** — is it essential, non-inferable, non-duplicative?

**Only unique information** — what Claude doesn't know, or what's specific to this use case:

```markdown
## Data Processing Requirements

Use pandas with these constraints:
- Preserve original column order (df.reindex())
- Handle timestamps in Pacific timezone
- Round currency values to 2 decimal places
```

**Workflows with checklists** for complex tasks:

```markdown
1. **Analyze Input**
   - [ ] Validate format
   - [ ] Check required fields
2. **Process Data**
   - [ ] Apply transformations
   - [ ] Validate output
```

**Feedback loops** where quality matters:

```markdown
1. Run validation
2. If errors found → review → fix → return to step 1
3. If passes → proceed
```

## Scripts and Code Guidelines

- **Error handling**: scripts give specific, actionable messages (what failed, which rows, how to fix)
- **File paths**: forward slashes only — `data/input/file.csv`
- **Dependencies**: documented, with configuration values explained (no magic numbers)
- **MCP tools**: fully qualified names (`ServerName:tool_name`)

See [reference/scripts-and-code.md](./reference/scripts-and-code.md) for comprehensive guidance.

## Phase 4: Quality Checklist

**Structure:**

- [ ] Valid YAML frontmatter; name follows conventions (gerund, ≤64 chars, lowercase/hyphens)
- [ ] Description includes usage triggers (≤1024 chars, third person)
- [ ] SKILL.md under 500 lines; supporting files one level deep
- [ ] Frontmatter options match invocation intent (Phase 1, question 3)

**Content:**

- [ ] Only information Claude doesn't already have
- [ ] Consistent terminology; concrete examples; checklists on complex workflows
- [ ] No time-sensitive information

**Technical:**

- [ ] Scripts have explicit error handling
- [ ] File paths use forward slashes
- [ ] Dependencies documented

## Phase 5: Verify the Skill Triggers Correctly

A skill that never fires (or fires on the wrong prompts) is a silent failure. Before calling it done, sanity-check the `description` against realistic prompts:

- **Should-trigger:** name 2–3 prompts a user would actually type for this task. Does the description's wording and keywords make Claude reach for the skill? If a plausible request wouldn't surface it, the description is too narrow or missing keywords.
- **Should-NOT-trigger:** name 1–2 adjacent prompts this skill should ignore (e.g. a generic question, a neighboring skill's job). Does the description risk over-firing or shadowing another skill? Tighten the scope if so.

For a non-trivial skill, run one real invocation (or, in a forked subagent, dry-run the workflow against a realistic input) and fix what breaks via the correcting-mistakes skill before shipping. Deploying an unexercised skill is deploying untested instructions.

## Reference Files

- [Scripts and Code](./reference/scripts-and-code.md) — error handling, dependencies, configuration
- [Architecture Diagrams](./reference/architecture-diagrams.md) — skill architecture diagram guidelines
