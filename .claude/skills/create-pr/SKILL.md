---
name: create-pr
description: Create a GitHub PR from the current branch with auto-generated title and description
disable-model-invocation: true
allowed-tools: Bash, Read, Glob, Grep, Agent
argument-hint: [base-branch]
---

# Create Pull Request

Create a GitHub Pull Request from the current branch, auto-generating the title and description from commits and code changes.

## Steps

### 1. Validate Branch State

Get the current branch:
```
git branch --show-current
```

Determine the base branch: use `$ARGUMENTS` if provided, otherwise default to `main`.

**Check for issues:**
- If on the base branch, stop and tell the user to switch to a feature branch.
- If there are uncommitted changes, warn the user and ask if they want to continue or commit first.
- If a PR already exists for this branch, show its URL and ask if they want to update it instead.

Check for an existing PR:
```
gh pr view --json url,state 2>/dev/null
```

### 2. Analyze Changes

Get commits ahead of the base branch:
```
git log <base-branch>..HEAD --oneline
```

Get the full diff:
```
git diff <base-branch>...HEAD --stat
```

For a detailed understanding, read the actual diff:
```
git diff <base-branch>...HEAD
```

Use Glob, Grep, Read, and Agent (with subagent_type=Explore) as needed to understand the context of the changes.

### 3. Detect Labels

Fetch available labels from the repository:
```
gh label list
```

Based on the nature of the changes, suggest appropriate labels (e.g., `enhancement`, `bug`, `documentation`).

### 4. Generate PR Draft

Auto-generate:
- **Title** — concise (under 70 characters), reflecting the main change
- **Description** — structured as:
  ```
  ## Summary
  - Bullet points describing key changes

  🤖 Generated with [Claude Code](https://claude.com/claude-code)
  ```
- **Labels** — list of suggested labels

Present the draft to the user for review. Ask if they want changes to the title, description, or labels.

Wait for user approval before proceeding.

### 5. Push and Create PR

Push the branch to remote if not already pushed:
```
git push -u origin <current-branch>
```

Create the PR using `gh`:
```
gh pr create --title "<title>" --body "<body>" --label "<label1>" --label "<label2>"
```

### 6. Confirm

Show the user:
- The PR URL
- The title, labels, and base branch
