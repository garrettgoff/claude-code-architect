---
description: Initialize progressive decomposition architecture documentation in your repository
---

# Initialize Architecture

You are helping the user bootstrap architectural documentation for their codebase using the progressive decomposition pattern.

## Process

1. **Check current state**: Look for existing ARCHITECTURE.md files in the repository
2. **Create root documentation**: If no root ARCHITECTURE.md exists, create one at the repository root
3. **Scan for components**: Identify major directories that represent architectural components
4. **Propose component docs**: Present a list of candidates and ask which ones to document
5. **Create component docs**: Generate ARCHITECTURE.md files for approved components
6. **Verify consistency**: Ensure all parent/child references are correct

## Key Principles

- Keep initial documentation lightweight - capture structure, not every detail
- Focus on Purpose, External Interfaces (Provides/Requires), and Internal Architecture
- Use the progressive decomposition pattern: each component can have subcomponents
- Maintain bidirectional references (parent → child, child → parent)

## Template Structure

Each ARCHITECTURE.md should include:
- **Purpose**: What this component does and why it exists
- **External Interfaces**: What it provides (ports) and what it requires (dependencies)
- **Internal Architecture**: Subcomponents and internal connections
- **Parent reference**: Link back to parent component
- **Last Updated**: Timestamp for tracking freshness

Use the architecture-init skill to guide this process.
