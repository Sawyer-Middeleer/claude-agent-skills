# Dot Claude

A collection of actually useful skills and subagents for Claude Code, focused on research, analysis, and skill creation.

## Installation

Add the marketplace to your Claude Code instance:

```claude
/plugin marketplace add Sawyer-Middeleer/dot-claude
```

Then install the plugins:

```claude
/plugin install dot-claude-skills
/plugin install dot-claude-agents
```

**Important:** Both plugins are required for full functionality. Some of the skills and agents rely on each other.

## What's Included

### dot-claude-skills

#### 1. `creating-skills`
Creates high-quality Claude skills following official best practices, with some additional quality of life improvements. Includes guided scoping dialog, progressive disclosure patterns, and quality validation.

#### 2. `conducting-deep-research`
Conducts comprehensive research on complex topics with technical rigor, synthesizing multiple sources including academic papers, technical documentation, industry reports, and practitioner insights. Supports both adaptive (sequential) and parallel (fast) research modes.

**Note:** This skill requires the `research-analyst` subagent (from dot-claude-agents) to function properly. Do not use this skill without installing both plugins.

#### 3. `analyzing-source`
Conducts in-depth analysis of specific sources, producing comprehensive summaries for research synthesis. Creates detailed, information-dense summaries suitable for integration into larger research projects.

**Note:** This skill is meant to be used by the `research-analyst` subagent (from dot-claude-agents) within the conducting-deep-research workflow.

### dot-claude-agents

#### `research-analyst`
Expert research analyst that investigates specific sources as part of a larger research project. Performs deep analysis and creates comprehensive summaries. This subagent is spawned by the `conducting-deep-research` skill to handle parallel source analysis.

**Technical details:**
- Model: Haiku (optimized for speed and cost)
- Skills: `analyzing-source`
- Permission Mode: default

## Dependencies

- The `conducting-deep-research` skill depends on the `research-analyst` subagent
- The `research-analyst` subagent uses the `analyzing-source` skill
- All three components work together as an integrated research system
