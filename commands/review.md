---
name: review
description: Start a comprehensive code review with multiple expert perspectives for OpenSpec-driven projects
argument-hint: "[path] [--full]"
allowed-tools: ["Read", "Grep", "Glob", "Task"]
---

# Vibe Review Command

You are executing the `/review` command to start a comprehensive code review for an OpenSpec-driven vibe coding project.

## Arguments

- `[path]` - Optional: Specific directory or file to review (default: entire project)
- `--full` - Optional: Force full project review regardless of size

## Execution Process

### Step 1: Project Discovery

First, understand the project structure:

1. Check if `openspec/` directory exists
2. List contents of `openspec/specs/` to understand specifications
3. Check `openspec/changes/` for pending changes
4. Identify main source directories (src/, lib/, app/, etc.)
5. Determine technology stack from file extensions and config files

If no `openspec/` directory exists, inform the user:
> "This project doesn't appear to have OpenSpec specifications. Would you like me to proceed with a general code review, or should you initialize OpenSpec first with `openspec init`?"

### Step 2: Determine Scope

Based on arguments and project size:

- **If path specified**: Review only that path
- **If --full specified**: Review entire project
- **If project small (<50 source files)**: Review entire project
- **If project large**: Suggest focusing on specific areas or recent changes

### Step 3: Launch Expert Agents

Use the Task tool to launch expert agents in parallel:

1. **Architect Agent** (`subagent_type: "vibe-review:architect"`)
   - Prompt: "Analyze the architecture of [scope] for consistency, layering violations, and dependency issues. Reference specifications in openspec/specs/ when available."

2. **Code Quality Expert** (`subagent_type: "vibe-review:code-quality-expert"`)
   - Prompt: "Analyze code quality of [scope] for duplication, complexity, code smells, and naming issues. Focus on issues common in AI-generated code."

3. **Spec Guardian** (`subagent_type: "vibe-review:spec-guardian"`)
   - Prompt: "Check specification compliance of [scope] against openspec/specs/. Identify coverage gaps, deviations, and orphan code."

### Step 4: Synthesize Results

After all agents complete:

1. Collect findings from each agent
2. Deduplicate overlapping issues
3. Assign unique IDs to each issue (ARCH-XXX, CQ-XXX, SPEC-XXX)
4. Prioritize by severity (Critical > High > Medium > Low)

### Step 5: Present Review Report

Format the combined report:

```markdown
# Vibe Review Report

**Project**: [project name]
**Scope**: [reviewed scope]
**Date**: [current date]

## Executive Summary

| Category | Critical | High | Medium | Low | Total |
|----------|----------|------|--------|-----|-------|
| Architecture | X | X | X | X | X |
| Code Quality | X | X | X | X | X |
| Spec Compliance | X | X | X | X | X |
| **Total** | X | X | X | X | X |

## Critical Issues (Requires Immediate Attention)

[List critical issues with details]

## Architecture Issues

[From architect agent]

## Code Quality Issues

[From code-quality-expert agent]

## Specification Deviations

[From spec-guardian agent]

## Recommendations

### High Priority
1. [Action item]
2. [Action item]

### Medium Priority
1. [Action item]

### Low Priority
1. [Action item]
```

### Step 6: Request Approval

After presenting the report:

> "Review complete. I found [X] issues across [Y] categories.
>
> **Next Steps:**
> - Review the findings above
> - Let me know which issues to address (e.g., 'focus on critical issues' or 'address ARCH-001, CQ-003')
> - Say 'generate proposals' to create OpenSpec change proposals for approved items
> - Say 'dismiss [issue-id]' to mark false positives
>
> What would you like to do?"

## Error Handling

- If no source code found: "No source code found in the specified path."
- If OpenSpec not initialized: Suggest running `openspec init`
- If agent fails: Report partial results and note which analysis failed

## Tips

- For large projects, start with `--full` only if you have time for comprehensive review
- Focus on critical and high severity issues first
- Use `/generate-proposal` after review to create actionable refactoring plans
