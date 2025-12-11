---
name: OpenSpec Format Reference
description: |
  This skill provides OpenSpec specification format knowledge. Use when creating change proposals, writing spec deltas, understanding OpenSpec structure, or generating compliant documentation. Triggers on questions about "openspec format", "proposal template", "spec delta", "change proposal structure", or "openspec syntax".
version: 1.0.0
---

# OpenSpec Format Reference

This skill provides comprehensive knowledge of OpenSpec format for spec-driven AI development.

## OpenSpec Directory Structure

```
openspec/
├── AGENTS.md           # Instructions for AI coding assistants
├── specs/              # Current source-of-truth specifications
│   ├── overview.md     # System overview
│   ├── auth.md         # Authentication specification
│   ├── orders.md       # Orders specification
│   └── ...
└── changes/            # Proposed changes (WIP)
    └── feature-name/
        ├── proposal.md
        ├── tasks.md
        ├── design.md   # Optional
        └── specs/
            └── *.delta.md
```

## Specification File Format

### Basic Structure

```markdown
# [Specification Name]

## Overview

[Brief description of this specification's scope]

## Requirements

### REQ-[PREFIX]-001: [Requirement Name]

[Requirement description using SHALL/MUST/SHOULD/MAY]

#### Scenarios

##### Scenario: [Scenario Name]
Given [precondition]
When [action]
Then [expected outcome]

##### Scenario: [Another Scenario]
Given [precondition]
When [action]
Then [expected outcome]

---

### REQ-[PREFIX]-002: [Next Requirement]

[Continue pattern...]
```

### Requirement Keywords

| Keyword | Meaning | Priority |
|---------|---------|----------|
| SHALL | Mandatory requirement | Must implement |
| MUST | Mandatory requirement | Must implement |
| SHOULD | Recommended requirement | Should implement |
| MAY | Optional requirement | Can implement |
| SHALL NOT | Prohibited action | Must not implement |
| MUST NOT | Prohibited action | Must not implement |

### Requirement ID Format

Pattern: `REQ-[AREA]-[NUMBER]`

Examples:
- `REQ-AUTH-001` - Authentication requirement
- `REQ-ORD-015` - Orders requirement
- `REQ-PAY-003` - Payment requirement
- `REQ-API-042` - API requirement

## Change Proposal Structure

### proposal.md Template

```markdown
# [Change Title]

## Summary

[1-2 sentence description of what this change accomplishes]

## Motivation

[Why is this change needed? What problem does it solve?]

### Problem Statement

[Detailed description of the current issue]

### Proposed Solution

[High-level description of the solution]

## Scope

### In Scope
- [Item 1]
- [Item 2]

### Out of Scope
- [Explicitly excluded item 1]
- [Explicitly excluded item 2]

## Impact

### Benefits
- [Benefit 1]
- [Benefit 2]

### Risks
- [Risk 1]: [Mitigation]
- [Risk 2]: [Mitigation]

### Breaking Changes
- [Any breaking changes, or "None"]

## Dependencies

- [Prerequisite 1]
- [Related proposal 1]
```

### tasks.md Template

```markdown
# Implementation Tasks

## Prerequisites

- [ ] Proposal reviewed and approved
- [ ] Dependencies resolved
- [ ] Test environment ready

## Phase 1: [Phase Name]

### [Task Group 1]

- [ ] Task description
  - Files: `path/to/file.py`
  - Spec: REQ-XXX-001
  - Notes: [Additional context]

- [ ] Another task
  - Files: `path/to/other.java`

### [Task Group 2]

- [ ] Task in group 2

## Phase 2: [Next Phase]

- [ ] Phase 2 tasks...

## Verification

- [ ] All tests pass
- [ ] Spec compliance verified
- [ ] Code review completed
- [ ] Documentation updated

## Post-Implementation

- [ ] Archive this change: `openspec archive [change-name]`
- [ ] Update affected specs in openspec/specs/
- [ ] Notify stakeholders
```

### Spec Delta Format

File: `specs/[spec-name].delta.md`

```markdown
# [Spec Name] Delta

## Source Specification
`openspec/specs/[spec-name].md`

---

## ADDED Requirements

### REQ-[PREFIX]-[NEW]: [New Requirement Name]

[New requirement description]

#### Scenarios

##### Scenario: [New Scenario]
Given [context]
When [action]
Then [outcome]

---

## MODIFIED Requirements

### REQ-[PREFIX]-[EXISTING]: [Requirement Name]

**Previous:**
```
[Original requirement text]
```

**Updated:**
```
[New requirement text]
```

**Rationale:**
[Why this change is needed]

#### Scenarios

##### Scenario: [Modified Scenario] (MODIFIED)
Given [updated context]
When [updated action]
Then [updated outcome]

##### Scenario: [New Scenario] (ADDED)
Given [new context]
When [new action]
Then [new outcome]

---

## REMOVED Requirements

### REQ-[PREFIX]-[OLD]: [Deprecated Requirement]

**Reason:** [Why this requirement is being removed]

**Migration:** [How existing code should handle this removal]

**Removed Scenarios:**
- [Scenario 1]
- [Scenario 2]
```

## AGENTS.md Template

```markdown
# AI Coding Assistant Instructions

## Project Overview

[Brief project description]

## Specifications

All specifications are in `openspec/specs/`. Read relevant specs before implementing.

## Active Changes

Check `openspec/changes/` for work in progress.

## Coding Standards

- [Standard 1]
- [Standard 2]

## Workflow

1. Read relevant specifications before coding
2. Create change proposal for new features
3. Reference spec requirements in code comments
4. Update specs when implementation differs
```

## Best Practices

### Writing Requirements

1. **Be specific** - Avoid ambiguous language
2. **Use active voice** - "System SHALL validate" not "Validation should be done"
3. **Include scenarios** - Show expected behavior with examples
4. **Keep atomic** - One requirement per ID, not compound requirements

### Writing Proposals

1. **Clear motivation** - Explain WHY before WHAT
2. **Explicit scope** - State what's NOT included
3. **Measurable tasks** - Each task should be completable and verifiable
4. **Link to specs** - Reference requirement IDs

### Writing Deltas

1. **Show before/after** - For modifications, show both versions
2. **Explain rationale** - Why is this change needed?
3. **Update scenarios** - Reflect new behavior
4. **Migration notes** - For removals, explain transition

## Common Patterns

### Feature Addition Flow

1. Create `openspec/changes/add-[feature]/`
2. Write proposal.md with motivation
3. Write tasks.md with implementation steps
4. Write specs/[area].delta.md with ADDED section
5. Implement following tasks
6. Archive when complete

### Bug Fix Flow

1. Create `openspec/changes/fix-[issue]/`
2. Write proposal.md describing bug and fix
3. Write tasks.md with fix steps
4. Write specs/[area].delta.md if behavior changes
5. Implement and verify
6. Archive when complete

### Refactoring Flow

1. Create `openspec/changes/refactor-[area]/`
2. Write proposal.md with refactoring rationale
3. Write tasks.md with step-by-step refactoring
4. Usually no spec delta (behavior unchanged)
5. Implement incrementally
6. Archive when complete
