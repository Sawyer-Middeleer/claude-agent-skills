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
5. **Research mode**: How should research be conducted? (use AskUserQuestion with single select)
   - Adaptive (sequential) - Each source informs the next. Better for exploratory research where you want to follow emerging threads and adapt strategy based on what you learn.
   - Parallel (fast) - All sources researched simultaneously. Faster execution but less adaptive. Best when research angles are well-defined upfront.

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

---

## Research Execution: Choose Based on Mode

After completing Phase 2 (Planning & Approval), proceed based on the research mode selected in Phase 1:

- **If Adaptive (sequential) mode selected**: Proceed to **Phase 3: Iterative Research & Progressive Synthesis** (below)
- **If Parallel (fast) mode selected**: Proceed to **Phase 3-Parallel: Parallel Research & Synthesis** (see section after Phase 3)

---

## Phase 3: Iterative Research & Progressive Synthesis

**Use this phase when: User selected Adaptive (sequential) research mode**

**IMPORTANT: Build synthesis progressively, rather than at the end. After every source, update synthesis and adapt approach.**

### Initial Setup

1. Create synthesis file from template: `./templates/research-synthesis.md`
2. Save to: `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/synthesis.md`
3. Initialize with research scope and empty theme sections
4. Create summaries directory: `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/summaries/`

### For Each Source (Sequential Loop)

#### Step 1: Discover and Fetch Next Source

Based on current synthesis state:

- Identify the highest-priority gap or angle to explore
- Use WebSearch to find relevant source
- Use WebFetch to retrieve the source content
- Prioritize sources that:
  - Fill identified gaps in current synthesis
  - Validate or challenge emerging patterns
  - Provide missing perspectives from research angles

#### Step 2: Create Source Summary

- Use template from `./templates/article-summary.md`
- Analyze the fetched source thoroughly
- Save to `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/summaries/source-title-slug-summary.md`

#### Step 3: Update Synthesis Immediately (KEEP CONCISE)

**CRITICAL: The synthesis document must remain concise and well-structured throughout. Add only essential insights.**

Update `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/synthesis.md` with findings from this source:

- **Add 1-3 key insights** to relevant theme sections (not everything from the source)
- **Update patterns/contradictions** only if source significantly confirms or challenges findings
- **Cite source summaries**: `[[filename]]` (summaries include links to original source URLs)
- **Refine gaps section** (mark gaps filled, identify new gaps)
- **Note important connections** to previous sources (only significant ones)

**Writing discipline:**

- Extract the essence, not the details (details live in source summaries)
- Each insight should be 1-2 sentences maximum
- Combine overlapping insights from multiple sources rather than listing separately
- Remove or consolidate redundant information as you add new sources
- The synthesis should be increasingly refined, not increasingly bloated

#### Step 4: Evaluate & Adapt

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

#### Step 5: Continue or Conclude

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

---

## Phase 3-Parallel: Parallel Research & Synthesis

**Use this phase when: User selected Parallel (fast) research mode**

### Overview

In parallel mode, research is conducted by spawning multiple researcher subagents simultaneously—one for each source topic. Each researcher independently fetches, analyzes, and creates a comprehensive summary of their assigned source. After all researchers complete, you synthesize their findings into a unified research document.

**Key differences from adaptive mode:**
- All sources researched simultaneously (faster)
- No iterative adaptation based on findings from each source
- Commit to initial research plan without mid-research pivots
- More comprehensive individual summaries required (since main agent won't read original sources)
- Final synthesis happens after all research is complete

### Step 1: Setup

1. Create synthesis file from template: `./templates/research-synthesis.md`
2. Save to: `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/synthesis.md`
3. Create summaries directory: `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/summaries/`

### Step 2: Extract Research Topics

From the approved research plan, identify specific source topics or search queries to research. Aim for target source count from Phase 1 (6-8 for Light, 9-14 for Medium, 15+ for Deep).

**For each research angle in the plan**, identify 2-4 specific sources/topics to investigate:

Example:
- Research Angle: "Performance patterns in distributed systems"
  - Topic 1: "Kubernetes horizontal pod autoscaling best practices"
  - Topic 2: "Netflix microservices scaling strategies"
  - Topic 3: "Load balancing algorithms for distributed systems"

Create a list of N source topics to research (where N = target source count).

### Step 3: Spawn Researcher Subagents in Parallel

**CRITICAL: Use a single message with multiple Task tool calls to execute all researchers in parallel.**

For each source topic, spawn a researcher subagent using the Task tool:

```
Task tool parameters for each researcher:
- subagent_type: "general-purpose"
- description: "Research [brief topic description]"
- prompt: "You are executing the researcher subagent role defined in /agents/research-analyst.md.

Your assignment:
- Source topic: [specific topic or URL]
- Research focus: [specific research angle from plan]
- Research purpose: [overall research goal from Phase 1]
- Working directory: [YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]

Follow the instructions in /agents/research-analyst.md to:
1. Find and fetch the source (use WebSearch then WebFetch, or direct WebFetch if URL provided)
2. Conduct deep analysis
3. Create comprehensive summary using analyzing-source/templates/article-summary-parallel.md
4. Save to: [YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/summaries/[descriptive-filename].md

Provide a brief confirmation when complete with the key insights discovered."
```

**Execute all N researchers in parallel by including N Task tool calls in a single message.**

### Step 4: Wait for Completion

All researcher subagents will execute in parallel. Wait for all to complete before proceeding.

### Step 5: Review Researcher Outputs

Once all researchers have completed:

1. List all summary files created in `[YOUR WORKING DIRECTORY]/[RESEARCH TOPIC]/summaries/`
2. Read each source summary file to understand what each researcher discovered
3. Note the range of perspectives, key themes, contradictions, and gaps

### Step 6: Build Comprehensive Synthesis

Now synthesize all findings into the `synthesis.md` file:

**Synthesis approach:**

1. **Identify major themes** across all sources
   - Look for patterns that appear in multiple sources
   - Note areas of consensus and disagreement
   - Group related findings together

2. **Organize synthesis by themes**, not by source
   - Each theme section should integrate insights from multiple sources
   - Compare and contrast different perspectives
   - Build coherent narrative for each theme

3. **For each theme, include:**
   - Core insights and findings (citing source summaries using `[[filename]]`)
   - Supporting evidence and examples from multiple sources
   - Areas of agreement across sources
   - Contradictions or debates (with citations)
   - Practical implications
   - Remaining gaps or open questions

4. **Draw evidence-based conclusions**
   - Only make claims supported by multiple sources
   - Note confidence level based on source quality and consensus
   - Identify areas requiring further research

5. **Create executive summary**
   - Synthesize key findings across all themes
   - Highlight most important insights
   - Note limitations of the research

**Synthesis writing principles:**

- Be comprehensive yet concise—capture all important insights without redundancy
- Cite liberally using `[[source-filename]]` format
- Make connections explicit between different sources and themes
- Distinguish between well-supported findings and tentative conclusions
- Note source quality in your assessments (per the "Evidence Quality Assessment" sections in summaries)

### Step 7: Quality Review

Verify synthesis quality:

- [ ] All major themes from source summaries are represented
- [ ] Synthesis is organized thematically, not source-by-source
- [ ] Key findings are supported by citations to source summaries
- [ ] Contradictions and debates are identified and explained
- [ ] Conclusions are proportional to evidence strength
- [ ] Executive summary captures essential findings
- [ ] Writing is clear, precise, and information-dense
- [ ] All internal links work
- [ ] YAML frontmatter is complete

### Step 8: Deliver Results

**Inform user:**
- Location of synthesis and all source summaries
- Summary of major findings and themes
- Note any limitations (inaccessible sources, areas with thin coverage, etc.)
- Highlight any unexpected or particularly significant discoveries

### Notes on Parallel Mode

**Advantages:**
- Much faster execution (all sources researched simultaneously)
- Good for well-defined research questions with clear angles
- Efficient use of compute resources

**Tradeoffs:**
- Less adaptive—can't pivot based on what early sources reveal
- May miss emergent themes that would guide sequential search
- Requires well-defined source topics upfront
- No progressive clarification during research

**Best suited for:**
- Time-sensitive research needs
- Well-understood topics with clear structure
- Situations where research angles are predetermined
- Cases where breadth is more important than depth

---

## Progressive Clarification

**Note: Progressive Clarification only applies to Adaptive (Phase 3) mode. In Parallel mode, commit to the initial plan.**

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
    ├── summaries/
    │   ├── example-source-1-summary.md
    │   └── example-source-2-summary.md
    ├── plan.md
    └── synthesis.md
```
