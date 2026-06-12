---
name: analyzing-source
description: Conducts in-depth analysis of a specific source or topic, producing a comprehensive, information-dense summary for research synthesis. Use when detailed analysis and documentation of an individual source is needed as part of a larger research effort. Primarily used by the research-analyst agent inside the deep-research workflow.
---

# Analyzing a Source

Analyze a single source in depth and create a comprehensive summary suitable for research synthesis.

## Process

### Step 1: Retrieve

- **Given a URL:** fetch it directly with WebFetch; verify the content is accessible and relevant
- **Given a topic:** WebSearch for the best source, prioritizing authoritative and detailed ones; fetch the most relevant result
- **Inaccessible or thin:** try alternative sources; note access issues in the summary rather than fabricating around them

### Step 2: Deep Analysis

Focus on:

- **Core concepts, definitions, frameworks** the source presents
- **Main arguments, claims, findings** — what is it asserting?
- **Evidence, data, examples** — what supports the claims?
- **Methodology** — how was the work conducted?
- **Limitations, caveats, counterarguments** — where are the boundaries?
- **Connections to the research focus** provided
- **Quality and credibility** — how reliable is this source, and why?
- **Unique insights** — what does this source offer that others won't?

### Step 3: Write the Summary

Use the template at `${CLAUDE_PLUGIN_ROOT}/skills/analyzing-source/templates/article-summary.md`.

Be concise yet thorough — extremely information-dense, leveraging specific data:

- Include direct quotes and concrete examples, not just paraphrase
- Provide analytical judgment about significance, not just description
- Connect explicitly to the research focus
- Make the summary self-contained — readable without the original

### Step 4: Save

- Filename: a descriptive slug (`kubernetes-scaling-patterns.md`, `pricing-research-meta-analysis.md`)
- Location: `{working_directory}/summaries/{filename}.md`
- All template sections filled

### Step 5: Report

1. What source was analyzed
2. Where the summary was saved
3. 2–3 sentences on the most important insights

## Guidelines

- **Thorough, not brief** — this is deep research, not scanning; capture nuance
- **Specific evidence** — quotes, data points, named examples
- **Critical** — note limitations, assess quality, surface assumptions
- **Focused** — comprehensive within the research focus, not encyclopedic beyond it
- **Always save the file** — the coordinator depends on it existing
