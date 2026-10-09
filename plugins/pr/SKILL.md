---
name: pr
description: Open a pull request for the current work. Use when the user asks for a PR, to "open a PR", or to "send this up for review". Pushes a branch and opens the PR but never merges it.
model: sonnet
---

# Open a Pull Request

Take the work in the current repository and get it in front of the user as a pull request. The user always makes the final merge decision.

## Rules

- Never merge a pull request, enable auto-merge, or approve one on the user's behalf. Open it and stop.
- Never push to `main` (or the repository's default branch) directly.
- Never force-push, and never skip hooks or signing.
- If a push or PR creation fails, report the exact error. Do not work around a blocked push by faking a remote ref or pushing somewhere else.

## Steps

1. **Check the state.** Run `git status` and `git diff` against the default branch. If there is nothing to ship, say so and stop. If there are unrelated changes mixed in, ask before including them.
2. **Branch.** If on the default branch, create a descriptive branch (`feature/<topic>`, `fix/<topic>`). Otherwise stay on the current branch. Before pushing, fetch the default branch and rebase if the branch is behind.
3. **Verify.** Run the repo's own checks (tests, lint, build) if they are quick to run, and mention any failures in the PR description rather than hiding them.
4. **Commit.** Use focused commits with imperative-mood subjects explaining the why. Do not commit secrets or `.env` files.
5. **Push** with `git push -u origin <branch>`.
6. **Open the PR** against the default branch, using `gh pr create` or a connected GitHub tool. Mark it as a draft if it is incomplete or blocked on something the user must do.

## PR description

Keep it short and useful to a reviewer:

- **Summary:** what changed and why, in a few sentences.
- **Testing:** what was run and the result, or what was not run and why.
- **Notes:** anything the reviewer should look at closely, follow-ups, or manual steps required before or after merging.

## Finish

Report the PR link and a one-line summary. State that it is ready for the user's review and merge.
