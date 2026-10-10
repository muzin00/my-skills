---
name: pr-merge
description: Squash merge the current branch's GitHub pull request and return to its updated base branch. Use only when the user explicitly requests merging.
---

# Pull Request Merge

Invocation authorizes merging and branch cleanup without intermediate confirmation.

1. Require a clean Git worktree and attached branch. Read `gh pr view --json number,url,state,isDraft,reviewDecision,statusCheckRollup,baseRefName`. Stop for a missing or non-open PR, draft, requested changes, or failed/cancelled checks.
2. If checks are pending, report them and stop. Wait only when the user explicitly requests it: use `gh pr checks <number> --watch --fail-fast`, then refresh the PR once and apply the same stop conditions. GitHub enforces conflicts and repository protections at merge time.
3. Run `gh pr merge <number> --squash --delete-branch`. Never use `--admin` or bypass protections. Confirm with `gh pr view <number> --json state`; if still queued or awaiting merge, report that and leave the local branch alone.
4. After confirmed merge, ensure the active branch is `baseRefName`. If `gh` already switched and successfully updated it, skip redundant checkout and pull. Otherwise switch as needed and run `git pull --ff-only`. Report the PR URL and active branch; report any cleanup/update failure separately from merge success.

On failure, stop without force, discarding changes, or destructive workarounds.

