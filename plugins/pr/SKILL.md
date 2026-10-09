---
name: pr
description: Open a pull request for the current work, bumping the project version from Conventional Commits when warranted. Use when the user asks for a PR, to "open a PR", or to "send this up for review". Pushes a branch and opens the PR but never merges it.
model: sonnet
---

# Open a Pull Request

Take the work in the current repository and get it in front of the user as a pull request. The user always makes the final merge decision.

## Rules

- Never merge a pull request, enable auto-merge, or approve one on the user's behalf. Open it and stop.
- Never push to `main` (or the repository's default branch) directly.
- Never force-push, and never skip hooks or signing.
- Never create git tags or releases. Only edit version fields in files.
- If a push or PR creation fails, report the exact error. Do not work around a blocked push by faking a remote ref or pushing somewhere else.

## Steps

1. **Check the state.** Run `git status` and `git diff` against the default branch. If there is nothing to ship, say so and stop. If there are unrelated changes mixed in, ask before including them.
2. **Branch.** If on the default branch, create a descriptive branch (`feature/<topic>`, `fix/<topic>`). Otherwise stay on the current branch. Before pushing, fetch the default branch and rebase if the branch is behind.
3. **Verify.** Run the repo's own checks (tests, lint, build) if they are quick to run, and mention any failures in the PR description rather than hiding them.
4. **Commit.** Use focused commits written as [Conventional Commits](https://www.conventionalcommits.org/) (`type(scope): subject`, imperative mood, explaining the why). Do not commit secrets or `.env` files.
5. **Bump the version** if the rules in "Version bump" below call for it, as its own commit.
6. **Push** with `git push -u origin <branch>`.
7. **Open the PR** against the default branch, using `gh pr create` or a connected GitHub tool. If `gh pr create` fails because GraphQL is blocked, use the REST API (`gh api repos/{owner}/{repo}/pulls`). Mark it as a draft if it is incomplete or blocked on something the user must do.

## Version bump

Bump at most once per PR, using the highest level implied by the PR's commits (everything on the branch that is not on the default branch).

| Commit type                                        | Bump                  |
| -------------------------------------------------- | --------------------- |
| `!` after the type/scope, or a `BREAKING CHANGE:` footer | **major** (`X+1.0.0`) |
| `feat`                                             | **minor** (`X.Y+1.0`) |
| `fix`, `perf`, `refactor`                          | **patch** (`X.Y.Z+1`) |
| `docs`, `chore`, `ci`, `test`, `style`, `build`    | no bump               |

If the branch has commits that are not Conventional Commits, classify them yourself from the diff (new capability = feat, bug or small tweak = fix) rather than skipping the bump.

How to apply it:

1. **Find the version.** Look for a version the project actually publishes or tracks: `package.json`, `.claude-plugin/marketplace.json` (`metadata.version`) and `.claude-plugin/plugin.json`, `pyproject.toml`, `Cargo.toml`, a `VERSION` file. If several files carry the same version, bump them all together so they stay in sync.
2. **Skip when there is nothing to bump.** No version file found, the version is a placeholder (`0.0.0`, or a private app that never uses versions), or the PR already changes the version. In those cases do not invent a version scheme. Mention in the PR notes that no bump was made and why.
3. **Edit only the version fields**, then commit separately as `chore(release): bump version to X.Y.Z`.
4. **Say so in the PR.** Put the old version, new version, and the commit that drove the level in the Notes section, so the user can disagree with it.

## PR description

Keep it short and useful to a reviewer:

- **Summary:** what changed and why, in a few sentences.
- **Testing:** what was run and the result, or what was not run and why.
- **Notes:** anything the reviewer should look at closely, follow-ups, or manual steps required before or after merging. Include the version bump (or why there was none).

## Finish

Report the PR link and a one-line summary. State that it is ready for the user's review and merge.
