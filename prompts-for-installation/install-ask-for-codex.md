Install a reusable Codex skill called **Ask Mode**.

Goal: give me `$ask`, a strict read-only way to inspect and discuss a codebase without editing or implementing anything.

Do not assume you already know where Codex stores skills today, or which files it expects. Formats, folders and metadata files change between versions. Resolve them at install time, from the current official documentation and from my machine.

## Step 1 — Ask me first, then wait

Ask me which scope I want:

- **Personal / global:** available in all my projects.
- **Project only:** stored inside the current repository and versioned with it.

Wait for my answer. Create nothing before I reply.

If I choose personal/global, tell me that writing outside the current workspace may require my approval, and ask for it if your environment needs it.

## Step 2 — Find the current format and the current location

Before writing anything:

1. Consult the current official Codex documentation about skills. Follow redirects, and trust the page you actually land on rather than any path you remember or any path written in this prompt.
2. Look at my local environment: the installed Codex version, the folders it already scans for skills, and any built-in helper it offers for installing or listing skills.
3. From those two sources, determine the format Codex recommends **today** for a skill, which files it requires, and the exact location that matches the scope I chose.

Codex has used more than one personal skills folder over time, and public tutorials disagree. Do not pick one from memory. Resolve it, and if several candidate folders exist on my machine, show me what you found and ask me which one to use.

If the documentation and my local environment disagree, follow my local environment, and tell me about the difference.

## Step 3 — Show the resolved path before writing

Print the full resolved path of every file you intend to create, plus one sentence saying why that location matches the scope I chose. If the path is outside the current project, say so explicitly.

## Step 4 — Check what is already there

Check whether the skill already exists at the resolved location, including the case where it is a symbolic link — working or broken.

- If nothing exists, continue.
- If something exists, show me the differences with what you are about to write, and ask before replacing it. Never overwrite silently.
- If it is a broken symbolic link, tell me, and ask whether to remove it or to repair its target.

## Step 5 — Create the skill

Create the skill folder, its entry file, and any metadata file the current format requires — and nothing else.

Use the modern format only. Do not create a legacy or deprecated format by default. If a legacy format is still supported and you think it is useful for me, offer it as an explicitly optional extra, after the main installation, and only if I ask for it.

### Instructions the skill must contain, unchanged

Copy this text into the skill body exactly as written:

````md
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

### Behavior the metadata must express

The skill must be:

- named `ask`, so I invoke it by typing `$ask`;
- described so that Codex knows exactly when it applies, using this description:

  `Answer questions about a codebase in strict read-only mode. Inspect files, explain behavior or architecture, compare approaches, identify problems, and discuss hypothetical changes without editing files, changing git, installing dependencies, or implementing anything. Use only when the user explicitly invokes $ask or asks to stay in Ask Mode.`

- never triggered implicitly: it runs only when I invoke it myself;
- presented in the interface as **Ask Mode**, "Discuss code without changing anything."

The files below expressed exactly that behavior when this prompt was written. Treat them as a starting point to verify, not as the truth.

Entry file front matter:

```yaml
name: ask
description: Answer questions about a codebase in strict read-only mode. Inspect files, explain behavior or architecture, compare approaches, identify problems, and discuss hypothetical changes without editing files, changing git, installing dependencies, or implementing anything. Use only when the user explicitly invokes `$ask` or asks to stay in Ask Mode.
```

Interface and policy metadata, in the metadata file the current format expects and at the path it expects:

```yaml
interface:
  display_name: "Ask Mode"
  short_description: "Discuss code without changing anything."
  default_prompt: "Use $ask to answer my codebase question without making changes."
policy:
  allow_implicit_invocation: false
```

Check every field and every file name against the current documentation and against my installed version. Keep what is still valid, use the current name for anything that was renamed, and drop anything the current version rejects — telling me which ones you dropped and what I lose. Never invent a field or a file that does not exist: if a behavior above cannot be expressed anymore, say so plainly instead of faking it.

In particular: do not add any sandbox, permission, or approval field to the skill files unless the current documentation shows that such a field really exists. If it does not, say so, and instead tell me the flags the installed Codex CLI actually documents for starting a read-only session — check its built-in help rather than quoting flags from memory. That gives me a stronger technical boundary on top of the skill's instructions.

## Step 6 — Validate and explain

After writing:

1. Confirm the files exist where you said they would.
2. Confirm Codex actually sees the skill, using whatever check the current version offers.
3. Tell me whether I need to restart Codex or open a new session for it to appear.
4. Show me one concrete example, such as: `$ask how does authentication work in this project?`

## Constraints

- Do not modify any other file.
- Do not add dependencies, settings, or project configuration.
- Do not change the meaning of the read-only instructions above.
- Keep it simple: I am not a professional developer, so explain each choice in one plain sentence.
