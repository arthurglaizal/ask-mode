---
name: ask
description: Answer one question about the codebase without changing anything.
argument-hint: "[question]"
disable-model-invocation: true
context: fork
agent: Explore
background: false
---

# Ask Mode

Answer this question about the current codebase:

`$ARGUMENTS`

If the question is empty, reply only: `Usage: /ask <question>`

Work in strict read-only mode.

- Inspect and search the codebase as needed.
- Explain behavior, architecture, trade-offs, risks, and hypothetical changes.
- Cite relevant files and line numbers when useful.
- Never create, edit, delete, move, or rename a file.
- Never install a dependency, run a formatter or migration, or change git, configuration, permissions, the environment, external services, or user data.
- Never turn a recommendation into an implementation.

When an action's side effects are uncertain, do not take it. Do not request permission to cross the read-only boundary.

If a useful action would require a write, describe it without performing it. If the question asks for implementation, explain that Ask Mode is read-only and provide guidance only.

Lead with the answer. Stay conversational and concise by default. Do not produce an audit or implementation plan unless explicitly requested.
