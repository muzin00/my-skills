---
name: pr-merge
description: Merge the pull request for the current Git branch after validating its state and checks, then update its base branch. Use only when the user explicitly asks to merge the current branch's pull request.
---

# Pull Request Merge

Safely merge the pull request for the current branch and return to its updated base branch.

## Workflow

1. Confirm the current directory is a Git worktree with no uncommitted changes.
2. Run `gh pr view --json number,url,title,state,isDraft,mergeable,mergeStateStatus,reviewDecision,statusCheckRollup,baseRefName` for the current branch.
3. Stop and report the reason if there is no open pull request, it is a draft, it has requested changes, it has conflicts, or GitHub reports it as blocked.
4. Inspect all checks. If any are pending, run `gh pr checks --watch`, then refresh the pull request state. Stop and report any failed checks.
5. Run `gh pr merge --squash --delete-branch`. Do not use administrator privileges or bypass repository protections.
6. Verify that the pull request was merged. If it was queued instead, report that state and stop without changing the local branch.
7. Switch to the pull request's `baseRefName` and run `git pull --ff-only`.
8. Report the pull request URL and the active, updated base branch.

