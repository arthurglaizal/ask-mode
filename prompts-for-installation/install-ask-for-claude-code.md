Install a reusable Claude Code skill called **Ask Mode**.

Goal: give me an `/ask <question>` command that answers questions about the current codebase in strict read-only mode.

Do not assume you already know where Claude Code stores skills today, or which frontmatter fields it accepts. Formats, folders and field names change between versions. Resolve them at install time, from the current official documentation and from my machine.

## Step 1 — Ask me first, then wait

Ask me which scope I want:

- **Personal / global:** available in all my projects.
- **Project only:** stored inside the current repository and versioned with it.

Wait for my answer. Create nothing before I reply.

If I choose personal/global, tell me that writing outside the current workspace may require my approval, and ask for it if your environment needs it.

## Step 2 — Find the current format and the current location

Before writing anything:

1. Consult the current official Claude Code documentation about skills. Follow redirects, and trust the page you actually land on rather than any path you remember or any path written in this prompt.
2. Look at my local environment: the installed Claude Code version, the existing skill folders, and what this version actually supports.
3. From those two sources, determine the format Claude Code recommends **today** for a user-invocable command skill, and the exact location that matches the scope I chose.

If the documentation and my local environment disagree, follow my local environment, and tell me about the difference.

## Step 3 — Show the resolved path before writing

Print the full resolved path of every file you intend to create, plus one sentence saying why that location matches the scope I chose. If the path is outside the current project, say so explicitly.

## Step 4 — Check what is already there

Check whether the skill already exists at the resolved location, including the case where it is a symbolic link — working or broken.

- If nothing exists, continue.
- If something exists, show me the differences with what you are about to write, and ask before replacing it. Never overwrite silently.
- If it is a broken symbolic link, tell me, and ask whether to remove it or to repair its target.

## Step 5 — Create the skill

Create the skill folder and the entry file the current format requires, and nothing else.

Use the modern format only. Do not create a legacy or deprecated format by default. If a legacy format is still supported and you think it is useful for me, offer it as an explicitly optional extra, after the main installation, and only if I ask for it.

### Instructions the skill must contain, unchanged

Copy this text into the skill body exactly as written:

````md
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
````

If the placeholder used for the user's arguments has been renamed in the current version, use the current one, and tell me. Everything else in that text stays identical.

### Behavior the metadata must express

The skill must be:

- named `ask`, so I invoke it by typing `/ask`;
- described as: `Answer one question about the codebase without changing anything.`;
- invoked only by me, never automatically by Claude;
- hinted, in autocomplete, as taking a question as its argument;
- run in an isolated read-only exploration context, so it cannot inherit or affect the rest of the conversation;
- run in the foreground, so the answer arrives in the same turn as my question.

The metadata block below expressed exactly that behavior when this prompt was written:

```yaml
name: ask
description: Answer one question about the codebase without changing anything.
argument-hint: "[question]"
disable-model-invocation: true
context: fork
agent: Explore
background: false
```

Check every field against the current documentation and against my installed version. Keep the fields that are still valid, use the current name for any field that was renamed, and drop any field the current version rejects — telling me which ones you dropped and what I lose. Never invent a field that does not exist: if a behavior above cannot be expressed in metadata anymore, say so plainly instead of faking it.

## Step 6 — Validate and explain

After writing:

1. Confirm the files exist where you said they would.
2. Confirm Claude Code actually sees the skill, using whatever check the current version offers.
3. Tell me whether I need to restart Claude Code or open a new conversation for it to appear.
4. Show me one concrete example, such as: `/ask how does authentication work in this project?`

## Constraints

- Do not modify any other file.
- Do not add dependencies, settings, or project configuration.
- Do not change the meaning of the read-only instructions above.
- Keep it simple: I am not a professional developer, so explain each choice in one plain sentence.
