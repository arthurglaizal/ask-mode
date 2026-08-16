Create a reusable Codex skill called **Ask Mode**.

Goal: install `$ask`, a strict read-only way to inspect and discuss a codebase without editing or implementing anything.

Before creating anything, ask me where to install it:

- **Global (recommended):** `~/.agents/skills/ask/`, available in every project.
- **Project only:** `.agents/skills/ask/`, versioned with the current repository.

Wait for my answer. If I choose global, warn me that writing outside the current workspace may require approval.

Create `SKILL.md` and `agents/openai.yaml` in the selected skill folder.

Put exactly this content in `SKILL.md`:

````md
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
````

Put exactly this content in `agents/openai.yaml`:

```yaml
interface:
  display_name: "Ask Mode"
  short_description: "Discuss code without changing anything."
  default_prompt: "Use $ask to answer my codebase question without making changes."
policy:
  allow_implicit_invocation: false
```

Important: a Codex skill cannot set the session sandbox in `SKILL.md` or `agents/openai.yaml`. Do not invent such a field. After installing, explain that the user can launch Codex with `codex --sandbox read-only --ask-for-approval untrusted` for a stronger technical boundary.

Constraints:

- Do not modify any other file.
- Do not add dependencies or project configuration.
- Do not create legacy `.codex/prompts/` or `~/.codex/skills/` files.
- If the target already exists, show me the difference and ask before replacing it.

At the end, state the created files, the installation scope, and one example using `$ask <question>`.
