---
name: Code Review Patterns
description: |
  This skill provides knowledge about common code issues and review patterns. Use when analyzing code quality, identifying anti-patterns, detecting architecture violations, or reviewing AI-generated code. Triggers on questions about "code smells", "anti-patterns", "architecture violations", "AI code issues", or "review checklist".
version: 1.0.0
---

# Code Review Patterns

This skill provides comprehensive knowledge of code issues, anti-patterns, and review patterns for vibe coding projects.

## AI-Generated Code Issues

AI coding assistants often introduce specific patterns that need review attention.

### Common AI Coding Problems

| Issue | Description | Detection |
|-------|-------------|-----------|
| **Over-abstraction** | Unnecessary interfaces, factories, wrappers | Single implementation of interface |
| **Verbose implementations** | Could be simplified significantly | Long methods doing simple things |
| **Inconsistent style** | Mixed coding conventions | Different naming/formatting in same file |
| **Copy-paste variations** | Similar but not identical code | Near-duplicate code blocks |
| **Missing edge cases** | Happy path only | No error handling, no null checks |
| **Orphan code** | Generated but never used | Unused functions, dead imports |
| **Over-commenting** | Obvious code heavily commented | Comment restates what code does |
| **Magic values** | Hardcoded without explanation | Literal numbers/strings in logic |

### Detection Strategies

```
# Over-abstraction check
- Interface with single implementation
- Abstract class with single subclass
- Factory that always returns same type
- Wrapper that just delegates

# Inconsistency check
- camelCase mixed with snake_case
- Different indentation styles
- Inconsistent brace placement
- Mixed quote styles (single/double)

# Copy-paste detection
- Similar method signatures
- Repeated logic patterns
- Only variable names differ
- Same structure, different data
```

## Architecture Anti-Patterns

### Layer Violations

| Violation | Description | Impact |
|-----------|-------------|--------|
| **Skip Layer** | UI directly calls database | Bypasses business logic |
| **Circular Dependency** | A→B→C→A | Tight coupling, hard to test |
| **God Module** | One module everything depends on | Single point of failure |
| **Leaky Abstraction** | Implementation details exposed | Couples layers tightly |
| **Anemic Domain** | Domain objects are just data | Logic scattered elsewhere |

### Dependency Direction Rules

```
Correct Flow:
┌─────────────────┐
│  Presentation   │ ──→ Can depend on Business, Domain
├─────────────────┤
│    Business     │ ──→ Can depend on Domain, Repository interfaces
├─────────────────┤
│     Domain      │ ──→ Should have minimal dependencies
├─────────────────┤
│   Repository    │ ──→ Can depend on Domain
├─────────────────┤
│ Infrastructure  │ ──→ Can depend on all above
└─────────────────┘

Violations to detect:
- Domain importing from Presentation
- Repository containing business logic
- Infrastructure types in Domain
- Circular imports between layers
```

### Module Cohesion Issues

```
Signs of Low Cohesion:
- Class does unrelated things
- Methods don't use same fields
- Hard to name the class (And, Or, Manager, Helper, Util)
- Frequent changes for unrelated reasons

Signs of High Coupling:
- Many parameters passed between modules
- Modules know internal structure of others
- Changes cascade across modules
- Can't test module in isolation
```

## Code Smells Catalog

### Bloaters

| Smell | Detection | Threshold |
|-------|-----------|-----------|
| **Long Method** | Line count | >30 lines concerning, >50 critical |
| **Large Class** | Line count, method count | >300 lines, >20 methods |
| **Long Parameter List** | Parameter count | >4 parameters |
| **Data Clumps** | Same group of params repeated | 3+ occurrences |
| **Primitive Obsession** | Primitives for domain concepts | Should be value objects |

### Object-Orientation Abusers

| Smell | Detection | Example |
|-------|-----------|---------|
| **Switch Statements** | Type checking in switch/if | `if type == "A"` patterns |
| **Parallel Inheritance** | Creating one class requires another | UserDTO requires UserEntity |
| **Refused Bequest** | Subclass doesn't use parent methods | Empty override methods |
| **Alternative Classes** | Different classes, same interface | Should be unified |

### Change Preventers

| Smell | Detection | Impact |
|-------|-----------|--------|
| **Divergent Change** | One class changes for multiple reasons | Low cohesion |
| **Shotgun Surgery** | One change affects many classes | High coupling |
| **Parallel Hierarchies** | Adding class requires adding another | Unnecessary complexity |

### Dispensables

| Smell | Detection | Action |
|-------|-----------|--------|
| **Dead Code** | Never called/used | Remove |
| **Lazy Class** | Does too little | Merge into another |
| **Speculative Generality** | Unused flexibility | Remove until needed |
| **Duplicate Code** | Same/similar code multiple places | Extract to shared |
| **Comments** | Explain bad code instead of fixing | Refactor code |

### Couplers

| Smell | Detection | Issue |
|-------|-----------|-------|
| **Feature Envy** | Method uses another class more | Should be moved |
| **Inappropriate Intimacy** | Classes know too much about each other | High coupling |
| **Message Chains** | a.getB().getC().getD() | Law of Demeter violation |
| **Middle Man** | Class mostly delegates | Remove intermediary |

## Review Checklists

### Quick Review Checklist

```markdown
## Code Quality
- [ ] No obvious code duplication
- [ ] Methods under 50 lines
- [ ] No deep nesting (>3 levels)
- [ ] Meaningful variable names
- [ ] No magic numbers/strings

## Consistency
- [ ] Naming convention consistent
- [ ] Code style matches project
- [ ] Error handling consistent
- [ ] Logging consistent

## AI-Generated Code
- [ ] No over-abstraction
- [ ] No verbose implementations
- [ ] Edge cases handled
- [ ] No orphan/unused code
```

### Full Review Checklist

```markdown
## Architecture
- [ ] No layer violations
- [ ] No circular dependencies
- [ ] Proper separation of concerns
- [ ] Dependencies flow correctly
- [ ] No god modules

## Code Quality
- [ ] No code duplication (>10 lines)
- [ ] Methods focused (single responsibility)
- [ ] Classes cohesive
- [ ] No code smells
- [ ] Proper error handling

## Spec Compliance
- [ ] Requirements implemented
- [ ] Scenarios covered
- [ ] Naming matches spec
- [ ] No orphan features
- [ ] Changes documented

## Security
- [ ] Input validation
- [ ] Output encoding
- [ ] Authentication/authorization
- [ ] No hardcoded secrets
- [ ] Secure defaults

## Performance
- [ ] No N+1 queries
- [ ] Efficient algorithms
- [ ] Resource cleanup
- [ ] No memory leaks
- [ ] Appropriate caching
```

## Severity Classification

### Critical (Must Fix)

- Security vulnerabilities
- Data corruption risks
- Spec violations on MUST/SHALL
- Complete layer bypass
- Circular dependencies in core

### High (Should Fix Soon)

- Significant code duplication
- Major code smells
- Partial spec coverage
- Performance issues
- Missing error handling

### Medium (Fix When Possible)

- Minor code smells
- Style inconsistencies
- Missing documentation
- Minor naming issues
- Non-critical optimization

### Low (Nice to Have)

- Code formatting
- Comment improvements
- Minor refactoring
- Style preferences
- Documentation enhancements

## Technology-Specific Patterns

### Java Patterns

```
Watch for:
- Spring @Service in wrong layer
- JPA entities with business logic
- Checked exceptions overused
- Mutable state in services
- Missing @Transactional
```

### Python Patterns

```
Watch for:
- Circular imports
- Missing type hints
- Bare except clauses
- Global state mutation
- Non-Pythonic idioms
```

### General Patterns

```
All languages:
- Resource leak (files, connections)
- Null/None handling
- Exception swallowing
- Hardcoded configuration
- Missing input validation
```

## Refactoring Suggestions

### For Duplication
1. Extract Method - Pull common code into shared method
2. Extract Class - Create utility class for shared logic
3. Template Method - Use inheritance for variation
4. Strategy Pattern - Use composition for variation

### For Complexity
1. Extract Method - Break down long methods
2. Replace Conditional with Polymorphism
3. Introduce Parameter Object
4. Replace Nested Conditional with Guard Clauses

### For Coupling
1. Introduce Interface - Abstract dependencies
2. Dependency Injection - Invert control
3. Event-Driven - Decouple with events
4. Facade Pattern - Simplify interface
