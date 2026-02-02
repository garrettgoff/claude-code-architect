# Claude Code Architecture Plugin

A Claude Code plugin for maintaining modular software architecture using progressive decomposition and adversarial design review.

## What This Plugin Does

This plugin helps you maintain clean, modular architecture in your codebase through:

1. **Progressive Decomposition**: Document your architecture hierarchically with ARCHITECTURE.md files at each component level
2. **Adversarial Design Review**: Automated review that challenges proposals to prevent architectural decay
3. **Architecture-Aware Development**: Query your architecture, propose changes, and implement with architectural consistency

## Installation

### From Local Directory (Development)

```bash
claude --plugin-dir /path/to/claude-code-architect
```

### From Git Repository

Add to your project's `.claude/settings.json` or global settings:

```json
{
  "plugins": [
    {
      "type": "git",
      "url": "https://github.com/garrettgoff/claude-code-architect.git"
    }
  ]
}
```

Or install via command:

```bash
/plugin install git:https://github.com/garrettgoff/claude-code-architect.git
```

## Commands

Once installed, you'll have access to these commands (namespaced with `arch:`):

### `/arch:init`
Initialize architectural documentation in your repository. Creates root ARCHITECTURE.md and helps document major components. Best for new projects.

```bash
/arch:init
```

### `/arch:discover [path]`
Reverse-engineer architecture documentation from an existing codebase by analyzing code structure, dependencies, and organization. Best for existing projects.

```bash
/arch:discover              # Analyze entire repository
/arch:discover src/         # Analyze specific directory
```

### `/arch:query [query]`
Query and understand your current architecture.

```bash
/arch:query What does the auth component do?
/arch:query What depends on the database module?
/arch:query Where should I put user session management?
/arch:query Check architecture consistency
```

### `/arch:workflow [request]`
Run the full architecture workflow: gather context, propose design, adversarial review, update docs, and implement.

```bash
/arch:workflow Add user authentication
/arch:workflow Refactor the API layer
```

## Skills

The plugin also includes architecture skills that Claude can automatically invoke:

- **architecture-query**: Understand component structure, interfaces, and dependencies
- **architecture-proposer**: Propose architectural changes for feature requests
- **architecture-reviewer**: Validate proposals with adversarial review
- **architecture-init**: Bootstrap architecture documentation for new projects
- **architecture-discover**: Reverse-engineer architecture from existing codebases

## Architecture Pattern: Progressive Decomposition

This plugin uses the progressive decomposition pattern where:

1. Each component has an `ARCHITECTURE.md` file with frontmatter and structured sections:
   ```yaml
   ---
   managed-by: arch-plugin
   schema-version: 1.0.0
   doc-version: 1.2.3
   last-updated: 2026-02-01
   ---
   ```
   - **Purpose**: What it does and why
   - **External Interfaces**: What it provides (ports) and requires (dependencies)
   - **Internal Architecture**: Subcomponents and internal connections
   - **Parent reference**: Link to parent component

2. Components can be decomposed into subcomponents recursively
3. Single-file implementations don't need ARCHITECTURE.md (described in parent)
4. Interfaces are explicit and consistency is enforced
5. Semantic versioning tracks document evolution (see [ARCHITECTURE_SCHEMA.md](ARCHITECTURE_SCHEMA.md))

Example structure:
```
project/
├── ARCHITECTURE.md              # Root architecture
├── auth/
│   ├── ARCHITECTURE.md          # Auth component
│   ├── session/
│   │   └── ARCHITECTURE.md      # Session subcomponent
│   └── tokens.ts                # Single-file (in parent's doc)
└── api/
    └── ARCHITECTURE.md          # API component
```

## Workflow Examples

### New Project
1. **Initialize** architecture:
```bash
/arch:init
```

2. **Query** your architecture:
```bash
/arch:query Show me the auth component structure
```

3. **Make changes** with architecture workflow:
```bash
/arch:workflow Add OAuth2 support to authentication
```

The workflow will:
- Analyze current architecture
- Propose design changes
- Run adversarial review
- Update ARCHITECTURE.md files
- Implement the code changes
- Verify consistency

### Existing Project
1. **Discover** existing architecture:
```bash
/arch:discover
```

2. **Review** generated docs and refine uncertain areas

3. **Use workflow** for future changes:
```bash
/arch:workflow Refactor authentication system
```

## Philosophy

### Adversarial Design Review
Unlike typical code reviews that look for problems, this plugin uses **adversarial review** where the reviewer actively tries to find issues:
- Challenge interface decisions
- Question new dependencies
- Verify simpler alternatives were considered
- Push back on complexity

This constructive adversarialism catches architectural problems before they become technical debt.

### Progressive Disclosure
Documentation is progressive:
- Start with simple root ARCHITECTURE.md
- Add component docs as code grows
- Decompose components when they exceed single-file complexity
- Maintain consistency through bidirectional references

### Architecture-First Development
Major changes follow this flow:
1. Understand current architecture
2. Propose architectural changes
3. Get adversarial review
4. Update architecture docs
5. Implement code
6. Verify consistency

This prevents "code first, document later" which leads to drift.

## Development

To test changes locally:

```bash
cd claude-code-architect
claude --plugin-dir .
```

Make changes to skills or commands, then restart Claude Code to pick up updates.

## Plugin Structure

```
claude-code-architect/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── commands/                     # User-invoked commands
│   ├── discover.md
│   ├── init.md
│   ├── query.md
│   └── workflow.md
├── skills/                       # Model-invoked skills
│   ├── architecture-discover/
│   │   └── SKILL.md
│   ├── architecture-init/
│   │   └── SKILL.md
│   ├── architecture-query/
│   │   └── SKILL.md
│   ├── architecture-proposer/
│   │   └── SKILL.md
│   └── architecture-reviewer/
│       └── SKILL.md
├── ARCHITECTURE_SCHEMA.md       # Schema definition for ARCHITECTURE.md files
├── CONTRIBUTING.md
├── INSTALL.md
└── README.md
```

## License

MIT

## Contributing

Contributions welcome! Please ensure:
1. Skills have clear purpose and usage documentation
2. Commands provide good user experience
3. Architecture pattern principles are maintained
4. Examples are included for new features
