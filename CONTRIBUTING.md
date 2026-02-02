# Contributing to Claude Code Architecture Plugin

Thanks for your interest in contributing! This plugin helps maintain modular architecture through progressive decomposition and adversarial design review.

## Development Setup

1. Clone the repository:
```bash
git clone https://github.com/ggoff/claude-code-architect.git
cd claude-code-architect
```

2. Test locally with Claude Code:
```bash
claude --plugin-dir .
```

3. Make your changes to commands or skills

4. Restart Claude Code to pick up changes

## Plugin Structure

- `.claude-plugin/plugin.json` - Plugin manifest with metadata
- `commands/` - User-invoked commands (e.g., `/architecture:arch-init`)
- `skills/` - Model-invoked skills (Claude uses these automatically)

## Adding a New Command

1. Create a new `.md` file in `commands/`
2. Add frontmatter with description:
```markdown
---
description: Brief description of what this command does
---
```
3. Write the command instructions for Claude
4. Test with `claude --plugin-dir .`

## Adding a New Skill

1. Create a new directory in `skills/` (e.g., `skills/my-skill/`)
2. Create `SKILL.md` inside with frontmatter:
```markdown
---
name: my-skill
description: When Claude should use this skill. Be specific about triggers.
---
```
3. Write detailed skill instructions
4. Test by triggering the skill's usage conditions

## Skill Writing Guidelines

- **Description field**: Must clearly state when Claude should invoke the skill
- **Purpose section**: Explain what the skill does at a high level
- **When to Use**: Specific conditions and examples
- **Process**: Step-by-step instructions Claude should follow
- **Output**: What the skill produces
- **Special Cases**: Edge cases and error handling

## Command Writing Guidelines

- Commands should orchestrate skills, not duplicate skill logic
- Use clear phase-based workflows for complex commands
- Always explain what's happening to the user
- Provide examples of usage
- Handle the `$ARGUMENTS` placeholder for user input

## Testing

Test your changes by:

1. Running the command or triggering the skill in a test repository
2. Verifying output matches expected behavior
3. Testing edge cases (missing files, malformed input, etc.)
4. Ensuring consistency with architecture principles

## Architecture Principles

This plugin is built on these principles:

1. **Progressive Decomposition**: Components decompose hierarchically
2. **Explicit Interfaces**: All component boundaries are documented
3. **Adversarial Review**: Actively look for problems, not just validate
4. **Consistency Enforcement**: Parent/child docs must align
5. **Progressive Disclosure**: Start simple, add detail as needed

Contributions should reinforce these principles.

## Code of Conduct

- Be respectful and constructive
- Focus on improving architecture maintenance
- Provide examples and documentation for new features
- Test your changes before submitting

## Submitting Changes

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make your changes
4. Test thoroughly
5. Commit with clear messages: `git commit -m "Add feature: description"`
6. Push to your fork: `git push origin feature/my-feature`
7. Open a pull request with:
   - Description of changes
   - Motivation/use case
   - Testing performed
   - Example usage

## Questions?

Open an issue to discuss:
- New feature ideas
- Bugs or unexpected behavior
- Documentation improvements
- Architecture pattern questions
