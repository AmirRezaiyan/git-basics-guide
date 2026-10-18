# 4. Branching

## Learning objectives

Understand what branches represent, create and switch branches, inspect branch relationships, and remove branches safely.

## What is a branch?

A branch is a lightweight, movable reference to a commit. Branches let you develop a feature or fix separately from another line of work, then integrate it later. Creating a branch is usually fast and does not copy the entire repository into a second directory.

## List and inspect branches

```bash
git branch
git branch --all
git branch -vv
git status
```

`git branch` lists local branches; `--all` also shows remote-tracking branches; `-vv` displays upstream relationships when configured. The asterisk marks the current local branch in the standard branch list.

## Create and switch

Prefer the modern `git switch` command for branch navigation:

```bash
git switch -c feature/search-filter
```

This creates the branch and switches to it. The equivalent two-step sequence is:

```bash
git branch feature/search-filter
git switch feature/search-filter
```

Switch to an existing local branch with:

```bash
git switch main
```

The branch may be called `master`, `main`, `develop`, or something else. Use `git branch` to see the actual names; do not assume that every repository uses `main`.

## A practical branch workflow

```bash
git switch main
git pull --ff-only
git switch -c docs/add-stash-guide
# Edit files, then review and commit them.
git status
git diff
git add docs/
git diff --staged
git commit -m "Add stash guide"
```

The `git pull --ff-only` step is optional and works only when the current branch can be updated by fast-forward. If it fails, read the message and inspect your local and remote history rather than forcing an update. If there is no remote or no upstream configured, skip the pull.

## Rename a branch

```bash
git branch -m docs/add-stash-guide docs/stash-guide
```

To rename the current branch, simply run `git branch -m new-name`. Renaming a local branch does not automatically rename a branch on a remote host.

## Delete a branch

After the branch has been merged and you no longer need it:

```bash
git switch main
git branch -d docs/stash-guide
```

`-d` refuses to delete a branch that Git considers unmerged into the current branch. Use `git branch -D <branch>` only when you have verified that the branch's work is no longer needed; `-D` forces deletion of the local branch reference and can make commits harder to find.

## Branch naming examples

Choose names that communicate intent:

```text
feature/user-search
fix/login-redirect
docs/update-installation-guide
refactor/cache-service
```

Naming conventions are team-specific. Consistency matters more than any single convention.

## Common mistakes

- **Editing the wrong branch:** run `git status` before beginning work.
- **Deleting before checking integration:** inspect `git log --oneline --graph --all` and confirm the work is merged or otherwise saved.
- **Confusing a remote-tracking branch with a local branch:** `origin/feature/name` is a local reference to the remote's branch, not your active writable branch.
- **Switching with uncommitted work:** Git may carry non-conflicting edits across branches or block the switch if it would overwrite changes. Read the message and decide whether to commit, stash, or safely discard the edits.

## Summary

Use branches to isolate work. Create with `git switch -c`, change branches with `git switch`, inspect them with `git branch -vv`, and delete merged branches with `git branch -d`.

## Further reading

- [Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- [git-switch](https://git-scm.com/docs/git-switch)
- [git-branch](https://git-scm.com/docs/git-branch)
