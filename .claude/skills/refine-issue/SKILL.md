---
name: refine-issue
description: Refine a GitHub issue by exploring the codebase, asking questions, and enhancing the title and description
disable-model-invocation: true
allowed-tools: Bash, Read, Glob, Grep, Agent
argument-hint: <issue-number>
---

# Refine GitHub Issue

Refine a GitHub issue by combining its content with codebase context to produce a better title and description.

## Steps

### 1. Get the Issue

Use `$ARGUMENTS` as the issue number. If not provided, ask the user.

Fetch the issue:
```
gh issue view $ARGUMENTS
```

Note the current title and body.

### 2. Explore the Codebase

Based on the issue's title and description, explore the codebase to understand:
- What parts of the code are relevant to the issue
- Existing patterns, architecture, and conventions
- Related files, functions, or modules
- Any existing tests or documentation

Use Glob, Grep, Read, and the Agent tool (with subagent_type=Explore) as needed.

### 3. Ask Clarifying Questions

Present the user with targeted questions to fill gaps, such as:
- What is the motivation or use case?
- Are there specific acceptance criteria or constraints?
- Which parts of the codebase should be affected?
- Are there edge cases or dependencies to consider?
- What priority or scope is intended?

Wait for the user to answer before proceeding.

### 4. Draft the Refined Issue

Using the original issue, codebase context, and user answers, draft:
- **Refined title** — concise, specific, and actionable
- **Refined description** — structured with:
  - **Summary**: what and why
  - **Context**: relevant codebase details (files, modules, patterns)
  - **Requirements**: acceptance criteria or deliverables
  - **Notes**: edge cases, dependencies, or implementation hints

Present the draft to the user for review. Ask if they want changes.

### 5. Apply the Changes

Once approved, update the issue:
```
gh issue edit <number> --title "<new title>" --body "<new body>"
```

Confirm the update and show the issue URL.
