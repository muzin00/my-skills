---
name: issue-checkout
description: Create and switch to a Git branch derived from a GitHub issue. Use when the user wants to start work from an issue URL while following the repository's branch naming convention.
---

# Issue Checkout

Create a branch for a GitHub issue and switch the current repository to it.

## Workflow

1. Use the issue URL supplied by the user. If none was provided, ask for it before continuing.
2. Confirm the current directory is a Git worktree. If it has uncommitted changes, stop and tell the user so the changes are not carried onto another branch unintentionally.
3. Run `gh issue view <issue-url> --json title,url,labels` to retrieve the issue. Stop and report the error if it cannot be read.
4. When an `origin` remote exists, verify that it refers to the same repository as the canonical issue URL. Stop and report a mismatch.
5. Derive a concise branch name:
   - Follow the repository's documented or existing branch naming convention when one is clear.
   - Otherwise use `<type>/<description>`, choosing a type such as `feature`, `fix`, `refactor`, `chore`, or `docs` from the issue title and labels.
   - Translate the description to English when necessary, use lowercase kebab-case, and omit filler words.
6. Validate the name with `git check-ref-format --branch <branch-name>`.
7. If the branch exists locally, switch to it. Otherwise, create it from the current `HEAD` with `git switch -c <branch-name>`.
8. Report the active branch name and canonical issue URL.

Do not update a base branch, modify the issue, commit changes, push the branch, or open a pull request unless the user separately requests it.
