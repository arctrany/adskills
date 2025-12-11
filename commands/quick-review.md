---
name: quick-review
description: Quick code review focused on recent changes or specific files
argument-hint: "[files...]"
allowed-tools: ["Read", "Grep", "Glob", "Bash"]
---

# Quick Review Command

You are executing the `/quick-review` command for a fast, focused code review.

## Arguments

- `[files...]` - Optional: Specific files to review (space-separated)
- If no files specified, review recently modified files (git diff)

## Execution Process

### Step 1: Determine Files to Review

**If files specified:**
- Review the specified files directly

**If no files specified:**
- Try to get recently changed files using git:
  ```bash
  git diff --name-only HEAD~5
  ```
- If not a git repo, ask user which files to review

### Step 2: Quick Analysis

For each file, perform a rapid check:

1. **Code Quality Quick Scan**
   - Obvious code duplication
   - Very long methods (>50 lines)
   - Deep nesting (>4 levels)
   - Unused imports/variables

2. **Consistency Check**
   - Naming convention consistency
   - Style consistency with rest of codebase
   - Pattern consistency

3. **AI-Generated Code Patterns**
   - Over-verbose implementations
   - Unnecessary abstractions
   - Missing edge case handling
   - Inconsistent error handling

### Step 3: OpenSpec Quick Check

If `openspec/specs/` exists:
- Check if changed files relate to any specifications
- Flag any obvious spec deviations
- Note if changes should be documented in `openspec/changes/`

### Step 4: Present Quick Report

```markdown
# Quick Review Results

**Files Reviewed**: [count]
**Time**: [timestamp]

## Summary

| Severity | Count |
|----------|-------|
| Issues | X |
| Warnings | X |
| Suggestions | X |

## Findings

### [filename]

| Line | Type | Issue |
|------|------|-------|
| 45 | Issue | Long method (78 lines) |
| 102 | Warning | Deep nesting (5 levels) |

[Repeat for each file]

## Quick Fixes

These can be addressed immediately:
1. [Quick fix suggestion]
2. [Quick fix suggestion]

## Needs Deeper Review

These require full `/review` analysis:
- [Item requiring more investigation]
```

### Step 5: Offer Next Steps

> "Quick review complete.
>
> **Options:**
> - 'fix [issue]' - I'll help fix a specific issue
> - '/review [path]' - Run comprehensive review
> - 'create proposal' - Generate OpenSpec proposal for findings"

## Speed Optimization

This command prioritizes speed:
- No parallel agent dispatch (overhead too high for quick review)
- Direct file reading instead of extensive searching
- Focus on obvious issues, not deep analysis
- Skip architectural analysis (requires broader context)

## Use Cases

- Before committing changes
- Quick sanity check on AI-generated code
- Reviewing a single file or small change
- Pre-PR validation
