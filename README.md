# Vibe OpenSpec Review - AI Code Review Plugin for OpenSpec Projects

A Claude Code plugin that provides AI-assisted code review and refactoring for OpenSpec-driven vibe coding projects. Uses multiple expert perspectives to identify architectural inconsistencies, code quality issues, and specification deviations.

## Features

- **Multi-Expert Review**: Parallel analysis by specialized agents (Architect, Code Quality Expert, Spec Guardian)
- **OpenSpec Integration**: Full compatibility with OpenSpec specification-driven development
- **Automated Proposal Generation**: Creates OpenSpec-compliant refactoring proposals
- **Human Approval Workflow**: Review findings before generating proposals
- **AI Code Focus**: Specifically tuned to catch issues common in AI-generated code

## Installation

### Method 1: Install from GitHub Marketplace (Recommended)

The easiest way to install this plugin is through Claude Code's marketplace system:

```bash
# Add the GitHub repository as a marketplace source
claude plugin marketplace add https://github.com/arctrany/adskills

# Install the plugin from the marketplace
claude plugin marketplace install vibe-openspec-review

# Verify installation
claude plugin list
```

### Method 2: Direct GitHub Installation

Install directly from the GitHub repository:

```bash
# Install from GitHub URL
claude plugin install https://github.com/arctrany/adskills

# Or with specific branch/tag
claude plugin install https://github.com/arctrany/adskills#main
```

### Method 3: Local Installation

Clone and install locally for development or customization:

```bash
# Clone the repository
git clone https://github.com/arctrany/adskills
cd adskills

# Install from local directory
claude plugin install .

# Or test without installing (development mode)
claude --plugin-dir .
```

### Verify Installation

After installation, verify the plugin is loaded:

```bash
# List installed plugins
claude plugin list

# Check available commands
/help
```

You should see `/vibe-openspec-review:review`, `/vibe-openspec-review:quick-review`, and `/vibe-openspec-review:generate-proposal` in the available commands.

## Commands

### `/review [path] [--full]`

Start a comprehensive code review with multiple expert perspectives.

```bash
# Review entire project
/review

# Review specific directory
/review src/

# Force full review on large projects
/review --full
```

### `/quick-review [files...]`

Quick code review focused on specific files or recent changes.

```bash
# Review recent git changes
/quick-review

# Review specific files
/quick-review src/auth/login.py src/auth/register.py
```

### `/generate-proposal [issue-ids...] | all`

Generate OpenSpec-compliant refactoring proposals based on review findings.

```bash
# Generate proposals for specific issues
/generate-proposal ARCH-001 CQ-003

# Generate proposals for all issues
/generate-proposal all
```

## Expert Agents

### Review Coordinator
Orchestrates the review process, coordinates expert agents, and synthesizes findings into a unified report.

### Architect
Analyzes code architecture for:
- Layer violations
- Dependency direction issues
- Circular dependencies
- Module cohesion problems
- Pattern consistency

### Code Quality Expert
Checks code quality including:
- Code duplication
- Complexity metrics
- Code smells
- Naming conventions
- AI-generated code patterns

### Spec Guardian
Ensures specification compliance:
- Requirement coverage
- Implementation deviations
- Naming consistency with specs
- Orphan code detection
- Change synchronization

### Proposal Generator
Creates OpenSpec-compliant change proposals:
- Groups related issues
- Generates proposal.md, tasks.md
- Creates spec delta files
- Integrates with OpenSpec workflow

## Review Workflow

```
┌──────────────────┐
│    /review       │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Expert Analysis │ ← Architect, Code Quality, Spec Guardian
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Review Report   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Human Approval   │ ← User confirms findings
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│/generate-proposal│
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ OpenSpec Changes │ → openspec/changes/[proposal]/
└──────────────────┘
```

## OpenSpec Integration

This plugin integrates seamlessly with OpenSpec workflow:

1. **Reads Specifications**: Analyzes `openspec/specs/` for compliance checking
2. **Checks Changes**: Reviews `openspec/changes/` for pending work
3. **Generates Proposals**: Creates properly formatted change proposals
4. **Compatible Commands**: Works with `openspec apply` and `openspec archive`

## Requirements

- Claude Code CLI
- OpenSpec initialized project (recommended)
- Git repository (for quick-review)

## Project Structure

```
adskills/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── agents/
│   ├── review-coordinator.md
│   ├── architect.md
│   ├── code-quality-expert.md
│   ├── spec-guardian.md
│   └── proposal-generator.md
├── commands/
│   ├── review.md
│   ├── quick-review.md
│   └── generate-proposal.md
├── skills/
│   ├── openspec-format/
│   │   └── SKILL.md
│   └── review-patterns/
│       └── SKILL.md
└── README.md
```

## Example Usage

```bash
# Initialize OpenSpec in your project (if not already)
openspec init

# Run comprehensive review
/review

# Review findings and select issues to address
# ... review the report ...

# Generate proposals for critical issues
/generate-proposal ARCH-001 ARCH-002 SPEC-001

# Implement using OpenSpec
openspec apply refactor-auth-layer

# Archive when complete
openspec archive refactor-auth-layer
```

## Contributing

Contributions welcome! Please read the contributing guidelines and submit PRs.

## License

MIT License - See LICENSE file for details.
