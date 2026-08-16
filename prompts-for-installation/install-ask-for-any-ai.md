Create a reusable command, skill, rule, or equivalent called **Ask Mode** for the current AI coding assistant.

Goal: provide a portable read-only mode for asking questions about a codebase without edits or spontaneous implementation.

First inspect the current environment and identify its simplest supported reusable-instruction mechanism. Prefer its current native format; do not choose a deprecated mechanism as the primary installation.

If both user-level and project-level locations exist, ask me where to install it:

- **Global (recommended):** available in every project, because Ask Mode is a way of interacting with an agent.
- **Project only:** versioned with the current repository.

Wait for my answer before writing. If the environment only supports project scope, install there and explain why. Warn me before requesting permission to write outside the workspace.

The reusable instruction must enforce this behavior:

```md
Answer the user's question in strict read-only mode.

- Read and search files, inspect repository structure, and analyze existing code.
- Run a command only when it is certainly read-only and necessary for the answer.
- Explain behavior, architecture, trade-offs, risks, and hypothetical changes.
- Cite relevant files and line numbers when useful.
- Never create, edit, delete, move, or rename a file.
- Never apply a patch, install a dependency, run a formatter or migration, or execute a command that may write to disk.
- Never change git state, configuration, permissions, the environment, external services, or user data.
- Never turn a recommendation into an implementation.

When a command's side effects are uncertain, do not run it. Do not request permission to cross the read-only boundary.

If a useful action would require a write, describe it without performing it. If the user asks for implementation, explain that Ask Mode is read-only and provide guidance only.

Lead with the answer. Stay conversational and concise by default. Do not produce an audit or implementation plan unless explicitly requested.
```

Use one explicit invocation per read-only question. If the tool can technically restrict writes for that invocation without adding a broad or fragile configuration, use that supported mechanism. Otherwise, rely on the instruction and clearly state that the boundary is behavioral rather than sandbox-enforced.

Constraints:

- Do not modify unrelated files.
- Do not add dependencies.
- Do not change project configuration unless recognition of the native reusable instruction strictly requires it.
- If the target already exists, show me the difference and ask before replacing it.

At the end, state what was created, where it was installed, how to invoke it, and whether read-only behavior is technically enforced or instruction-based.
