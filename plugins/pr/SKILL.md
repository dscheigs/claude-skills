---
name: pr
description: Opens a pull request for the current branch in the user's preferred style, with a clear title and a description covering what changed, why, and what was not checked. Use whenever the user asks to open, create or write a PR.
model: sonnet
allowed-tools: Bash(git *) Bash(gh *)
---

# Open a pull request

The base branch is `main`. If the user names a different base, use it in place of `origin/main` below.

## Context

- Branch: !`git branch --show-current`
- Uncommitted: !`git status --short`
- Commits ahead: !`git log --oneline origin/main..HEAD || true`
- Files changed: !`git diff --stat origin/main...HEAD || true`

## Steps

1. Stop and tell the user if the branch is the base branch, or has no commits ahead of it. If there are uncommitted changes, say so and ask; do not commit them yourself.
2. Read the actual diff (`git diff <base>...HEAD`), not only the commit messages.
3. Push the branch with `git push -u origin HEAD`.
4. Open the PR with `gh pr create`. If that fails, fall back to `gh api repos/{owner}/{repo}/pulls` with the same title and body.
5. Reply with the PR URL and one sentence on what it does.

## Title

Match the style of recent commits (`git log -10 --oneline`). Under 70 characters, saying what changed, not how.

## Body

- **Summary**: one to three sentences on what this does and why.
- **Changes**: a short list, only when there is more than one thing.
- **Checked**: only what you actually ran, and the result.
- **Not checked**: anything untested or risky. Leave this out only if there is nothing.
- **Issues**: write `Closes #N` only if merging finishes that issue; otherwise `Part of #N`.

## Rules

- Never merge the PR or turn on auto-merge. The user merges.
- Never say something passed unless you ran it this session.
- Keep any attribution lines the session or repo requires at the end of the body.
