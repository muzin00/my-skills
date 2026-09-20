---
name: pr-create
description: Stage task-related changes, create an English Conventional Commit, push the current branch, and open an assigned GitHub pull request. Use when the user explicitly wants to commit completed work and create a pull request in one workflow.
---

# Pull Request Create

Commit completed work and create its pull request without separate confirmation steps.

## Workflow

1. Treat explicit invocation as authorization to stage relevant changes, commit, push, and create a pull request.
2. Confirm the current directory is a Git worktree. Determine the current branch and the repository's default branch with `gh repo view --json defaultBranchRef`. Stop if `HEAD` is detached or the current branch is the default branch.
3. Inspect `git status --short`, staged changes, and unstaged changes. Select only files related to the current task. Never stage unrelated files or files that may contain secrets.
4. Stage explicit paths with `git add -- <paths...>`. Do not use `git add .` or `git add -A`. If there are no eligible changes, stop and report it.
5. Review `git diff --cached` and run `git diff --cached --check`. Create one concise English Conventional Commit message that accurately describes the staged changes, then run `git commit`. Do not amend an existing commit.
6. Push without force. If the branch has no upstream, use `git push -u origin HEAD`; otherwise use `git push`.
7. Check whether the current branch already has an open pull request. If it does, report its URL and stop without creating a duplicate.
8. Fetch the remote default branch, then inspect the complete branch with `git log origin/<base>..HEAD --oneline` and `git diff origin/<base>...HEAD`.
9. Generate a pull request title of at most 70 characters and a concise body that follows the repository's language and style. Describe the full branch, not only the latest commit. Include validation results only for checks that were actually run.
10. Run `gh pr create` with the detected base branch and `--assignee "@me"`, then report the commit and pull request URL.

Do not ask for confirmation between steps. On failure, stop and report the cause; do not force push, bypass hooks or protections, discard changes, or use another destructive workaround.
