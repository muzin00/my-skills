---
name: issue-create
description: Draft and create a GitHub issue through a short interactive workflow. Use when the user explicitly wants to create an issue and its title, body, labels, or target repository may need clarification.
---

# Issue Create

Draft a clear GitHub issue and create it with GitHub CLI without a separate confirmation step.

## Workflow

1. Treat the user's arguments as initial issue context. Ask only for essential missing information: issue type, requested change or problem, and optional background or purpose.
2. Determine the target repository from the user's request or the current repository. Confirm it with `gh repo view --json nameWithOwner,url` before drafting.
3. Follow an applicable repository issue template when one exists. Check available labels with `gh label list`; use only existing labels and omit a label when none clearly applies.
4. Search open issues for a likely duplicate using a few distinctive title keywords. If a close match exists, stop and report it instead of creating a duplicate.
5. Draft a concise title and Markdown body in the repository's usual language and style. Do not invent requirements, reproduction steps, or acceptance criteria.
6. Treat the user's explicit request to create an issue as authorization to run `gh issue create` with the drafted values and `--assignee "@me"`. Do not ask for a separate confirmation; report the created issue URL.

Do not create labels, assign anyone other than the authenticated user, add the issue to projects or milestones, or modify another issue unless the user separately requests it.
