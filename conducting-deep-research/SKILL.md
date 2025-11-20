---
name: conducting-deep-research
description: Conducts comprehensive research on complex topics with technical rigor, synthesizing multiple sources including academic papers, technical documentation, industry reports, and practitioner insights. Use when the user needs structured, in-depth research with synthesis across diverse sources.
---

# Conducting deep research

Follow the research process described below.

## Phase 1: Initial Clarification

Use the **AskUserQuestion** tool to gather structured feedback on research parameters.

Ask the user to clarify:

1. **Research purpose**: What is the goal of this research/what angles should be explored? Suggest options contextually. (use AskUserQuestion with multiSelect: true)
2. **Scope boundaries**: What should be included/excluded? Any constraints? Suggest options contextually. (use AskUserQuestion with multiSelect: true)
3. **Source preferences**: Which types are most valuable? (use AskUserQuestion with multiSelect: true)
   - Academic papers and peer-reviewed research
   - Technical documentation / Industry whitepapers
   - Technical blogs from recognized experts
   - Practitioner discussions (Reddit, X, LinkedIn, HackerNews)
   - Company documentation and case studies
4. **Research depth**: How many sources? (use AskUserQuestion with options for Light: 6-8, Medium: 9-14, Deep: 15+)

## Phase 2: Planning & Approval

**Brief preliminary investigation:**

- Search for 2-3 representative sources
- Identify major themes, terminology, key authors/organizations
- Note unexpected angles or adjacent topics worth exploring

**Create research plan:**

1. Use template from `./templates/research-plan.md`
2. Ask the user where they would like you to save your work or if you should create a new working directory
3. Create `/[RESEARCH TOPIC]` inside the working directory
4. Create and save your research plan to `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/plan.md`

**Plan structure:**

- **Research angles**: 3-5 core sub-questions or perspectives
- **Source strategy**: Mix of source types with target quantities based on user preferences
- **Analysis approach**: Synthesis method (thematic, comparative, chronological)
- **Expected deliverables**: Final format and supporting documentation
- **Potential challenges**: Known gaps, access issues, complexity factors

## Phase 3: Iterative Research & Progressive Synthesis

**IMPORTANT: Build synthesis progressively, rather than at the end. After every source, update synthesis and adapt approach.**

### Initial Setup

1. Create synthesis file from template: `./templates/research-synthesis.md`
2. Save to: `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/synthesis.md`
3. Initialize with research scope and empty theme sections

### For Each Source (Sequential Loop)

#### Step 1: Discover Next Source

Based on current synthesis state:

- Identify the highest-priority gap or angle to explore
- Use WebSearch to find relevant source
- Prioritize sources that:
  - Fill identified gaps in current synthesis
  - Validate or challenge emerging patterns
  - Provide missing perspectives from research angles

#### Step 2: Fetch & Save Source

- Create `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/sources` if it doesn't already exist
- Web articles: Use WebFetch, save to `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/sources/source-title-slug.md`
- PDFs: Use Bash (curl/wget), save to `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/sources/source-title-slug.pdf`

#### Step 3: Create Source Summary

- Use template from `./templates/article-summary.md`
- Save to `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/source-title-slug-summary.md`
- Link to original using: `[[filename]]`

#### Step 4: Update Synthesis Immediately (KEEP CONCISE)

**CRITICAL: The synthesis document must remain concise and well-structured throughout. Add only essential insights.**

Update `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/synthesis.md` with findings from this source:

- **Add 1-3 key insights** to relevant theme sections (not everything from the source)
- **Update patterns/contradictions** only if source significantly confirms or challenges findings
- **Cite source summaries**: `[[filename]]` (source summaries already link to originals)
- **Refine gaps section** (mark gaps filled, identify new gaps)
- **Note important connections** to previous sources (only significant ones)

**Writing discipline:**

- Extract the essence, not the details (details live in source summaries)
- Each insight should be 1-2 sentences maximum
- Combine overlapping insights from multiple sources rather than listing separately
- Remove or consolidate redundant information as you add new sources
- The synthesis should be increasingly refined, not increasingly bloated

#### Step 5: Evaluate & Adapt

After updating synthesis, explicitly evaluate:

**What have we learned?**

- What themes are emerging strongly?
- What contradictions or debates are appearing?
- What gaps remain most critical?

**Should we adapt the approach?**

- Are research angles still valid or should we pivot?
- Which source types are proving most valuable?
- Should we dig deeper on specific sub-topics vs broadening?
- Have we found enough evidence for key claims?

**What to search for next?**

- Determine next highest-priority angle/gap
- Adjust search strategy based on what's working
- Consider whether we're approaching target source count

#### Step 6: Continue or Conclude

- If more sources needed and target count not reached: return to Step 1
- If sufficient coverage achieved: proceed to Phase 4 (Packaging & Delivery)
- If unexpected angle discovered: use Progressive Clarification (see below)

### Synthesis Principles

- **Organize by themes**, NOT by source
- **Update progressively** after each source, don't wait for the end
- **Keep concise** - synthesis should be information-dense and focus on key points
- **Identify patterns** as they emerge (2+ sources confirming)
- **Track contradictions** when sources disagree
- **Note gaps** in current understanding
- **Draw evidence-based conclusions** only when sufficient sources support them
- **Citation format:**
  - Source summaries: `[[filename]]`
  - With display text: `[[filename|display text]]`
  - External URLs: `[display text](https://url.com)`

### Quality Checks During Research

- [ ] Synthesis remains concise and readable
- [ ] Contradictions identified and explained
- [ ] Synthesis references all key sources
- [ ] Conclusions are evidence-based
- [ ] Writing is clear, precise, information-dense
- [ ] All internal links work
- [ ] YAML frontmatter complete

**Inform user:** Share location, summarize findings, note limitations.

## Progressive Clarification

If you discover important angles not in original plan:

1. Pause and inform user
2. Ask if scope should expand
3. Update plan if approved
4. Continue with revised plan

## Source Quality Guidelines

**Include if:**

- Provides unique insights or data
- Represents authoritative voice
- Offers contrarian or edge perspectives
- Contains primary research or original analysis

## Target Working Directory Structure

```text
[YOUR WORKING DIRECTORY]/
└── [RESEARCH TOPIC]/
    ├── sources/
    │   ├── example-source-1.md
    │   └── example-source-2.pdf
    ├── example-source-1-summary.md
    ├── example-source-2-summary.md
    ├── plan.md
    └── synthesis.md
```
