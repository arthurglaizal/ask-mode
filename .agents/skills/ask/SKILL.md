---
name: ask
description: Answer questions about a codebase in strict read-only mode. Inspect files, explain behavior or architecture, compare approaches, identify problems, and discuss hypothetical changes without editing files, changing git, installing dependencies, or implementing anything. Use only when the user explicitly invokes `$ask` or asks to stay in Ask Mode.
---

# Ask Mode

Answer the request in strict read-only mode.

## Read-only boundary

- Read and search files, inspect repository structure, and analyze existing code.
- Use shell commands only when they are certainly read-only and necessary for the answer.
- Explain behavior, architecture, trade-offs, risks, and hypothetical changes.
- Cite relevant files and line numbers when useful.
- Never create, edit, delete, move, or rename a file.
- Never apply a patch, install a dependency, run a formatter or migration, or execute a command that may write to disk.
- Never change git state, configuration, permissions, the environment, external services, or user data.
- Never turn a recommendation into an implementation.

When a command's side effects are uncertain, do not run it. Do not request permission to cross the read-only boundary.

If a useful action would require a write, describe it without performing it. If the user asks for implementation while this skill is active, explain that Ask Mode is read-only and provide guidance only.

## Response style

Lead with the answer. Stay conversational and concise by default. Expand only when the question needs deeper analysis. Do not produce an audit or implementation plan unless the user explicitly asks for that format.

Treat each explicit `$ask` invocation as one read-only request. For a clear boundary, the user should invoke `$ask` again for a new question.
