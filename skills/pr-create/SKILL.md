---
name: pr-create
description: Commit completed task changes, push the current branch, and create or reuse its GitHub pull request. Use when the user explicitly requests PR creation, including already committed work.
---

# Pull Request Create

Invocation authorizes commit, push, and PR creation without intermediate confirmation. Reuse task context, reviewed diffs, and successful validation unless relevant changes require another check.

1. Read `git status --short` and `git branch --show-current`. Stop on detached HEAD or the default branch (use local `origin/HEAD`, falling back to `gh repo view --json defaultBranchRef`). Find an open PR with `gh pr list --head <branch> --state open --json url,baseRefName`; use its base, or the default for a new PR.
2. If task changes remain, stage only their explicit paths. Keep unrelated changes, including already staged ones, out of the commit without discarding them. Review the commit diff as needed, run `git diff --cached --check`, and commit with a concise English Conventional Commit message. Skip this step when work is already committed.
3. For a new PR only, fetch `origin <base>` and inspect `git log origin/<base>..HEAD --oneline` and `git diff origin/<base>...HEAD` to describe the whole branch. Stop if there are no commits.
4. Push without force (`git push`, or `git push -u origin HEAD` without an upstream). If an open PR exists, report its URL and stop.
5. Create a concise title (at most 70 characters) and body in the repository's style, including only validation actually performed. Write the body to a temporary file and use `gh pr create --base <base> --assignee "@me" --title <title> --body-file <file>`. Report the URL and any new commit.

Do not commit secrets, amend commits, bypass hooks, or discard work. On failure, report the cause and stop.
