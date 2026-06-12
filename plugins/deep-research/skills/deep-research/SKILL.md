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

Use **AskUserQuestion** to set parameters — but **only ask what `$ARGUMENTS` and the conversation haven't already answered.** A well-specified request ("a deep competitive scan of X across academic and practitioner sources, ~15 sources, saved to ./research") may answer all of these; in that case skip straight to Phase 2 and confirm your read-back in one line rather than running the dialog. Batch any genuinely open questions into a single AskUserQuestion call, including the save location (question 5):

1. **Research purpose** — what is the goal; which angles matter? Suggest options contextually. (multiSelect)
2. **Scope boundaries** — inclusions, exclusions, constraints? Suggest options contextually. (multiSelect)
3. **Source preferences** (multiSelect): academic / official documentation and whitepapers / expert blogs / practitioner discussions (Reddit, X, HN, LinkedIn) / company case studies
4. **Depth** (single select): Light 6–8 sources · Medium 9–14 · Deep 15+
5. **Save location** — where the research folder goes (default: ask, or use a knowledge base's research location if one is detected)

## Phase 2: Plan & Approve

**Brief preliminary scan:** search for 2–3 representative sources; identify major themes, terminology, key authors; note unexpected angles worth folding in.

**Create the plan** from `${CLAUDE_PLUGIN_ROOT}/skills/deep-research/templates/research-plan.md`:

1. Create `<working-directory>/<topic-slug>/` at the save location from Phase 1
2. Save the plan to `<topic-slug>/plan.md` with: 3–5 research angles, source strategy per the user's preferences, synthesis approach (thematic / comparative / chronological), expected deliverables, known challenges
3. Get the user's approval before executing — pivoting mid-research isn't supported, so the plan carries the weight

**Knowledge-base check:** only treat the location as a knowledge base if there's a positive signal it manages *notes* — a `conventions.md` describing note frontmatter/tags/links, or a CLAUDE.md with a "knowledge base"/notes-conventions section. A bare `CLAUDE.md` (the norm in code repos) is **not** a knowledge base — don't infer note conventions from it. When a real KB is detected, adopt its frontmatter, tag, and link conventions for every file this workflow writes, and use its research location if it names one.

## Phase 3: Execute

### Setup

1. Create `<topic-slug>/synthesis.md` from `${CLAUDE_PLUGIN_ROOT}/skills/deep-research/templates/research-synthesis.md`
2. Create `<topic-slug>/summaries/`

### Queries → validated URLs

1. For each research angle, derive 2–4 specific search queries (total = target source count)
2. WebSearch each query; pick the strongest result
3. **Validate every URL with a minimal WebFetch** ("extract the title and first paragraph") before spawning any analyst. Treat as inaccessible: dead links, and **paywalled pages** — if the fetch returns only a teaser, an abstract, or a login/subscribe wall instead of the substance, the analyst will get the same, so don't send it. Replace with an alternate, but **cap at ~2 alternates per query**; if none are accessible, drop that query and note the gap rather than looping indefinitely.
4. Record which angle each validated URL serves, and which queries ended with no accessible source

**Source quality bar — include only if:** unique insight or data · authoritative voice · contrarian/edge perspective worth representing · primary research or original analysis.

### Spawn analysts in parallel

**One message, N subagent-spawn calls (the Agent tool, historically "Task")** — all analysts run simultaneously:

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

### Reconcile before synthesizing

Don't trust the analysts' self-reported success. After they finish:

1. List `summaries/` and match files against the set of URLs/angles you spawned
2. For any analyst that saved nothing (or to the wrong path), **re-spawn it once**
3. If it fails again, record the source as a permanent gap — name it in `synthesis.md`'s limitations and in the Phase 4 report; don't silently shrink the research
4. Only proceed once every spawned source is either reconciled or recorded as a gap

### Synthesize

1. Read every summary in `summaries/`
2. Organize **by theme, not by source** — each theme section integrates insights across sources
3. Per theme: core findings with citations to summary files · evidence and examples · consensus · contradictions and debates · practical implications · open questions
4. Conclusions proportional to evidence — claims need multiple sources; note confidence and source quality
5. Executive summary last: key findings across themes, limitations of the research

Cite summaries liberally — use the knowledge base's link style if one was detected, otherwise relative markdown links (`[title](summaries/filename.md)`), which resolve in any markdown viewer (a bare `[[wikilink]]` only resolves inside Obsidian-style tools).

**Sources Consulted** — end `synthesis.md` with a bibliography: every source as `[title](original-url)` with its publication date where known, grouped by research angle, plus the recorded gaps. This is the shareable citation trail; local summary links alone are useless to anyone you send the doc to.

### Verify before finalizing

A structural synthesis isn't a verified one. Before delivering:

- Spot-check the highest-stakes claims (the ones a decision would rest on) against the original sources, not just the summaries — flag any the sources don't actually support
- Note where the evidence is thin or single-sourced rather than stating it with the same confidence as well-supported findings
- Run one gap pass: which planned angles ended up under-covered, and say so

### Quality review

- [ ] Every research angle **from `plan.md`** is represented or its gap is noted (check against the plan, not just against the summaries that exist)
- [ ] Organized thematically, not source-by-source
- [ ] Findings cite source summaries; contradictions identified and explained
- [ ] Conclusions proportional to evidence strength; thin/single-source claims marked
- [ ] Sources Consulted bibliography present with working original URLs
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
