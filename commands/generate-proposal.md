---
name: generate-proposal
description: Generate OpenSpec-compliant refactoring proposals based on review findings
argument-hint: "[issue-ids...] | all"
allowed-tools: ["Read", "Write", "Grep", "Glob", "Task"]
---

# Generate Proposal Command

You are executing the `/generate-proposal` command to create OpenSpec-compliant change proposals based on review findings.

## Arguments

- `[issue-ids...]` - Specific issue IDs to address (e.g., ARCH-001 CQ-003 SPEC-002)
- `all` - Generate proposals for all identified issues
- No arguments - Prompt user to select issues

## Prerequisites

This command should be run after `/review` has identified issues. If no review has been done:

> "No review findings found. Please run `/review` first to identify issues, then use this command to generate proposals."

## Execution Process

### Step 1: Collect Issues

**If issue IDs specified:**
- Locate the specified issues from the review context
- Validate that all IDs exist

**If 'all' specified:**
- Collect all issues from the most recent review
- Confirm with user before proceeding

**If no arguments:**
- List available issues and ask user to select:
  ```
  Available issues from last review:

  Critical:
  - ARCH-001: Layer violation in UserService
  - SPEC-001: Missing authentication requirement

  High:
  - CQ-001: Duplicate validation logic (3 locations)
  - ARCH-002: Circular dependency between Order and Payment

  Which issues should I create proposals for? (Enter IDs or 'all')
  ```

### Step 2: Group Issues

Analyze issues to group into coherent proposals:

- **By feature area**: Issues in same module → single proposal
- **By root cause**: Issues with same underlying problem → single proposal
- **By fix approach**: Issues fixable together → single proposal

Present grouping to user:
```
I'll group these issues into the following proposals:

1. "refactor-auth-layer" (ARCH-001, SPEC-001)
   - Fix authentication layer violations

2. "consolidate-validation" (CQ-001)
   - Extract duplicate validation to shared utility

3. "break-circular-deps" (ARCH-002)
   - Resolve Order-Payment circular dependency

Proceed with these groupings? (yes/modify)
```

### Step 3: Launch Proposal Generator

Use the Task tool to invoke the proposal-generator agent:

```
subagent_type: "vibe-review:proposal-generator"
prompt: |
  Generate OpenSpec-compliant proposals for these issue groups:

  Group 1: refactor-auth-layer
  Issues: ARCH-001, SPEC-001
  Details: [issue details]

  Group 2: consolidate-validation
  Issues: CQ-001
  Details: [issue details]

  Create proper OpenSpec change structure in openspec/changes/ with:
  - proposal.md (rationale and scope)
  - tasks.md (implementation checklist)
  - specs/*.delta.md (specification changes)
```

### Step 4: Create Proposals

Ensure `openspec/changes/` directory exists, then create each proposal:

For each proposal group:
1. Create directory: `openspec/changes/[proposal-name]/`
2. Write `proposal.md` with rationale and scope
3. Write `tasks.md` with implementation checklist
4. Write `specs/[affected-spec].delta.md` for each affected specification

### Step 5: Present Results

```markdown
# Proposal Generation Complete

## Proposals Created

| Proposal | Issues | Files Created |
|----------|--------|---------------|
| refactor-auth-layer | ARCH-001, SPEC-001 | 3 |
| consolidate-validation | CQ-001 | 2 |
| break-circular-deps | ARCH-002 | 3 |

## Created Structure

```
openspec/changes/
├── refactor-auth-layer/
│   ├── proposal.md
│   ├── tasks.md
│   └── specs/
│       └── auth.delta.md
├── consolidate-validation/
│   ├── proposal.md
│   └── tasks.md
└── break-circular-deps/
    ├── proposal.md
    ├── tasks.md
    └── specs/
        └── orders.delta.md
```

## Next Steps

1. **Review proposals**: Read each proposal.md to verify scope
2. **Adjust if needed**: Edit tasks.md to refine implementation steps
3. **Implement**: Use `/openspec:apply [proposal-name]` to start implementation
4. **Archive**: After completion, run `openspec archive [proposal-name]`
```

## Error Handling

- **No openspec directory**: Create `openspec/changes/` if it doesn't exist
- **Invalid issue ID**: Report which IDs weren't found
- **Write permission error**: Report and suggest manual creation

## Integration with OpenSpec

The generated proposals follow OpenSpec format and integrate with:
- `openspec list` - Lists pending changes including generated proposals
- `openspec apply [name]` - Implements a proposal
- `openspec archive [name]` - Archives completed proposal

## Tips

- Start with critical issues for immediate impact
- Group related issues to minimize proposal count
- Review generated tasks before implementation
- Use spec deltas to keep specifications in sync with code changes
