# 5. Merging Branches

## Learning objectives

Understand how merge integrates history, merge a feature branch, recognize fast-forward and three-way merges, and know what to do if a merge conflict occurs.

## What does merge do?

`git merge <branch>` integrates the named branch into the **currently checked-out branch**. The order matters: switch to the branch that should receive the work, then merge the source branch into it.

## Basic merge example

Assume the feature branch is named `feature/search-filter` and its work is ready to integrate into `main`:

```bash
git status
git switch main
git merge feature/search-filter
```

Before merging, ensure you understand the state of the working tree. If there are uncommitted changes, commit or stash them when appropriate; don't ignore a warning about local changes.

If Git can move `main` forward to the feature commit without creating a merge commit, it may perform a **fast-forward**. If the two branches have diverged, Git commonly creates a merge commit when the merge is successful. The result depends on the commit graph and options used.

## View the graph

```bash
git log --oneline --graph --decorate --all
```

Use the graph to understand where branches diverged and how a merge integrated them.

## Merge without committing immediately

For some workflows, `git merge --no-commit --no-ff <branch>` asks Git to prepare the merge without creating the merge commit yet, allowing you to inspect the staged result. This is an advanced option; it is not a replacement for understanding the normal merge flow.

## If the merge conflicts

Git pauses when it cannot automatically reconcile overlapping changes. Follow the steps in [Git Conflicts](10-git-conflicts.md). In short:

1. Run `git status` to identify conflicted files.
2. Edit each file, choosing or combining the correct content.
3. Stage each resolved file with `git add <file>`.
4. Complete the merge with `git commit` if Git has not already completed it automatically.

To abandon an in-progress merge before committing, use `git merge --abort` when possible. Check `git status` afterward. Do not use abort as a substitute for first understanding whether local changes existed before the merge.

## Delete a merged feature branch

After verifying the result and confirming the branch is no longer needed:

```bash
git branch --merged
git branch -d feature/search-filter
```

Run the deletion from a different branch, such as `main`. Git protects unmerged branches from ordinary `-d` deletion.

## Merge versus rebase

- **Merge** integrates histories and can preserve the fact that two lines of development came together.
- **Rebase** replays commits onto a new base, creating new commit IDs for the replayed commits.

Neither is universally best. Follow your team's workflow and avoid rewriting shared history unexpectedly. See [Rebasing](06-rebasing.md).

## Common mistakes

- **Merging in the wrong direction:** check `git status` and the current branch before running `git merge`.
- **Deleting before verifying:** inspect the history and the final files first.
- **Assuming every merge creates a merge commit:** a fast-forward may move the branch pointer without one.
- **Taking every conflict side automatically:** resolve according to intended behavior, not by blindly choosing “ours” or “theirs.”

## Summary

Switch to the destination branch, then merge the source branch. Check the graph and tests after integration, resolve conflicts deliberately, and delete the source branch only after its work is safe.

## Further reading

- [Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- [git-merge](https://git-scm.com/docs/git-merge)
