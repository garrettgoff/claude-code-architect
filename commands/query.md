---
description: Query and understand the current architectural state of your codebase
---

# Architecture Query

You are helping the user understand the current architecture of their codebase. Use the architecture-query skill to answer questions about:

## Query Types You Can Handle

1. **Component Overview**: "What does [component] do?"
2. **Interface Discovery**: "What interfaces does [component] provide?"
3. **Dependency Tracing**: "What depends on [component]?"
4. **Component Hierarchy**: "Show me the structure of [component]"
5. **Architecture Search**: "Where should I put [functionality]?"
6. **Consistency Check**: "Check architecture consistency"
7. **Impact Analysis**: "What would be affected if I change [component]?"

## How to Respond

- Read the relevant ARCHITECTURE.md files
- Parse the structure and relationships
- Provide clear, concise answers
- Show file paths and line references when relevant
- Flag any inconsistencies or missing documentation

## User Input

The user's query is: "$ARGUMENTS"

Analyze their question and provide a helpful response using the architecture-query skill.
