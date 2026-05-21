# Cherry Picks

This document explains how cherry picks are managed on release branches.

<!-- toc -->
- [When to Cherry Pick](#when-to-cherry-pick)
- [How to Cherry Pick](#how-to-cherry-pick)
- [Cherry Pick Review](#cherry-pick-review)
- [Troubleshooting](#troubleshooting)
<!-- /toc -->

## When to Cherry Pick

Cherry picks are used to backport changes from `main` to release branches. Only the following types of changes are eligible:

- **Security fixes** — Vulnerability patches
- **Critical bug fixes** — Data loss, crashes, hangs
- **Regression fixes** — Fixes for bugs introduced in recent releases
- **Documentation corrections** — Important doc updates

Feature additions and non-critical changes should not be cherry-picked.

## How to Cherry Pick

1. Ensure the original PR is merged into `main`
2. Check out the release branch:
   ```bash
   git checkout release-X.Y
   git pull origin release-X.Y
   ```
3. Cherry-pick the commit:
   ```bash
   git cherry-pick -x <commit-hash>
   ```
   The `-x` flag adds a reference to the original commit in the cherry-pick message.
4. Resolve any conflicts, then commit:
   ```bash
   git cherry-pick --continue
   ```
5. Push the branch and create a PR:
   ```bash
   git push origin release-X.Y
   ```

## Cherry Pick Review

Cherry pick PRs should:

- Reference the original PR in the description
- Include a clear justification for the backport
- Be reviewed by a maintainer of the target release branch
- Include the target release version in the title, e.g. `[release-1.0] Fix crash on startup`

## Troubleshooting

**Conflicts**: If the cherry-pick does not apply cleanly, manually resolve conflicts before continuing. Ensure the resulting change is functionally equivalent to the original.

**CI failures**: If the cherry-pick causes CI failures, investigate whether the fix depends on other changes in `main`. If so, those dependencies may also need to be cherry-picked.
