---
name: backlog
description: Manages deferred/blocked work items as GitHub Issues with labels and blocker tracking
---

# Backlog

Manages deferred work items as GitHub Issues. Use this whenever work is identified but can't be done yet — blocked by another feature, out of scope for the current PR, or deferred by design.

## Commands

### `/backlog add <description>`

Create a new backlog issue.

1. Determine the **title** — short, imperative (e.g., "Add .options() and .env() to createTestService")
2. Determine what **blocks** it — another feature, a design decision, a package change, etc.
3. Create a GitHub Issue using `gh issue create` with:
   - Label: `backlog`
   - Label: `blocked` (if there's a known blocker)
   - Body format:

```markdown
## What

<1-2 sentences describing the deferred work>

## Blocked by

<What needs to happen first. Reference issues/PRs if applicable, or describe the prerequisite.>
- [ ] <blocker 1>
- [ ] <blocker 2> (if multiple)

## Context

<Where this came up — PR number, conversation context, or design doc reference>
```

4. Print the issue URL when done.

### `/backlog list`

List open backlog items.

```bash
gh issue list --label backlog --state open
```

Show the results grouped by blocked/unblocked:
- **Blocked** — issues that also have the `blocked` label
- **Ready** — issues with `backlog` label but NOT `blocked`

### `/backlog unblock <issue-number>`

Remove the `blocked` label from an issue when its prerequisite is done.

```bash
gh issue edit <number> --remove-label blocked
```

### `/backlog close <issue-number>`

Close a backlog item when it's been implemented.

```bash
gh issue close <number>
```

## When to Use

- **During PR review:** When a reviewer or agent flags a missing feature that's out of scope
- **During implementation:** When you discover work that depends on something not yet built
- **During planning:** When a phase is deferred or descoped
- **After merging:** When unblocking a prerequisite, run `/backlog list` to check if anything is now ready

## Labels

Ensure these labels exist in the repo (create them if they don't):
- `backlog` — deferred work item
- `blocked` — waiting on a prerequisite

## Rules

- Every deferred item MUST have a backlog issue — don't just mention it in a PR comment and forget
- Always include what blocks the work — "blocked by X" is required context
- Keep titles actionable — "Add X to Y" not "X is missing"
- Reference the PR or conversation where the item was identified
