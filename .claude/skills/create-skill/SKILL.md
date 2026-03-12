---
name: create-skill
description: Create a new custom Claude Code skill (slash command) interactively
disable-model-invocation: true
allowed-tools: Read, Write, Bash, Glob
argument-hint: [skill-name]
---

# Skill Creator

Create a new custom Claude Code skill. If a skill name is provided as `$ARGUMENTS`, use it. Otherwise, ask the user.

## Steps

### 1. Gather Requirements

Ask the user (skip any answered by arguments):
- **Skill name** — lowercase, hyphens allowed, no spaces (e.g. `fix-issue`, `deploy`)
- **Description** — one-line summary of what it does (under 200 chars)
- **What should it do?** — detailed instructions for Claude when the skill runs
- **Who invokes it?** — user-invocable (default), model-invocable, or both
- **Scope** — project (`.claude/skills/`) or personal (`~/.claude/skills/`)
- **Tools needed** — which tools should be allowed without prompting (Read, Write, Edit, Bash, Glob, Grep, WebFetch, WebSearch, Agent, etc.)
- **Takes arguments?** — if yes, what arguments and how they're used
- **Runs in isolation?** — whether to use `context: fork` for subagent isolation

### 2. Validate

- Skill name: lowercase alphanumeric and hyphens only, max 64 chars
- Description: under 200 characters
- Check if a skill with that name already exists at the target scope

### 3. Generate SKILL.md

Build the YAML frontmatter from the gathered info. Include only non-default options:
- `name` and `description` are always included
- Add `disable-model-invocation: true` if user-only
- Add `user-invocable: false` if model-only
- Add `allowed-tools` if specified
- Add `argument-hint` if the skill takes arguments
- Add `context: fork` if it runs in isolation

Write clear, actionable markdown instructions in the body.

Use `$ARGUMENTS` or `$0`, `$1`, etc. for argument placeholders where applicable.

Use `!`command`` syntax for dynamic context injection where useful (e.g., `!`git branch --show-current``).

### 4. Write the Files

Create the skill directory and write SKILL.md:

```
<scope>/skills/<skill-name>/SKILL.md
```

### 5. Confirm

Show the user:
- The full path of the created skill
- The contents of SKILL.md
- How to invoke it: `/<skill-name>` or `/<skill-name> <args>`
- Suggest testing it immediately
