---
name: deep-research
description: Conducts comprehensive multi-source research on a topic - clarifies scope, plans research angles, spawns parallel analyst subagents to study each source in depth, and synthesizes findings into cited documentation. Use when the user wants thorough research, a literature review, a competitive scan, or says "deep research", "research this properly", "build me a research doc".
argument-hint: "[topic]"
---

# Deep Research

Systematic research workflow: clarify → plan → parallel source analysis → thematic synthesis. Deliverables are files (`plan.md`, `summaries/`, `synthesis.md`), not a chat reply — research that lands somewhere reusable.

## Phase 0: Topic

`$ARGUMENTS` is the topic. If empty, ask: "What topic or research question would you like to investigate?"

## Phase 1: Clarification

Use **AskUserQuestion** to set parameters:

1. **Research purpose** — what is the goal; which angles matter? Suggest options contextually. (multiSelect)
2. **Scope boundaries** — inclusions, exclusions, constraints? Suggest options contextually. (multiSelect)
3. **Source preferences** (multiSelect): academic / official documentation and whitepapers / expert blogs / practitioner discussions (Reddit, X, HN, LinkedIn) / company case studies
4. **Depth** (single select): Light 6–8 sources · Medium 9–14 · Deep 15+

## Phase 2: Plan & Approve

**Brief preliminary scan:** search for 2–3 representative sources; identify major themes, terminology, key authors; note unexpected angles worth folding in.

**Create the plan** from `${CLAUDE_PLUGIN_ROOT}/skills/deep-research/templates/research-plan.md`:

1. Ask the user where to put the work, then create `<working-directory>/<topic-slug>/`
2. Save the plan to `<topic-slug>/plan.md` with: 3–5 research angles, source strategy per the user's preferences, synthesis approach (thematic / comparative / chronological), expected deliverables, known challenges
3. Get the user's approval before executing — pivoting mid-research isn't supported, so the plan carries the weight

**Knowledge-base check:** if the working directory (or its ancestors) contains a knowledge base — a `conventions.md` or a CLAUDE.md describing note conventions — adopt its frontmatter, tag, and link conventions for every file this workflow writes, and use its preferred research location if it names one.

## Phase 3: Execute

### Setup

1. Create `<topic-slug>/synthesis.md` from `${CLAUDE_PLUGIN_ROOT}/skills/deep-research/templates/research-synthesis.md`
2. Create `<topic-slug>/summaries/`

### Queries → validated URLs

1. For each research angle, derive 2–4 specific search queries (total = target source count)
2. WebSearch each query; pick the strongest result
3. **Validate every URL with a minimal WebFetch** ("extract the title and first paragraph") before spawning any analyst; replace dead or paywalled URLs
4. Record which angle each validated URL serves

**Source quality bar — include only if:** unique insight or data · authoritative voice · contrarian/edge perspective worth representing · primary research or original analysis.

### Spawn analysts in parallel

**One message, N Task tool calls** — all analysts run simultaneously:

```
subagent_type: "deep-research:research-analyst"
description: "Research <brief topic>"
prompt: "Source URL: <validated url>
Research focus: <angle from plan>
Research purpose: <goal from Phase 1>
Working directory: <working-directory>/<topic-slug>

Analyze this source per your instructions and save your summary to
<working-directory>/<topic-slug>/summaries/<descriptive-filename>.md,
then report your key insights."
```

Wait for all analysts to complete.

### Synthesize

1. Read every summary in `summaries/`
2. Organize **by theme, not by source** — each theme section integrates insights across sources
3. Per theme: core findings with citations to summary files · evidence and examples · consensus · contradictions and debates · practical implications · open questions
4. Conclusions proportional to evidence — claims need multiple sources; note confidence and source quality
5. Executive summary last: key findings across themes, limitations of the research

Cite summaries liberally using the knowledge base's link style if one was detected, otherwise `[[summary-filename]]`.

### Quality review

- [ ] All major themes from the summaries represented
- [ ] Organized thematically, not source-by-source
- [ ] Findings cite source summaries; contradictions identified and explained
- [ ] Conclusions proportional to evidence strength
- [ ] Internal links resolve; frontmatter complete (per KB conventions if detected)

## Phase 4: Deliver

Report to the user:

- Where everything lives (`plan.md`, `synthesis.md`, `summaries/`)
- Major findings and themes, briefly
- Limitations: inaccessible sources, thin coverage, lower-quality evidence
- Anything unexpected or particularly significant

```
<working-directory>/<topic-slug>/
├── plan.md
├── synthesis.md
└── summaries/
    ├── <source-1>.md
    └── <source-2>.md
```
