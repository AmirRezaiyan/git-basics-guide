# 6. Rebasing

## Learning objectives

Understand how rebase replays commits onto a new base, perform a simple local rebase, resolve conflicts during rebase, and identify when rebase is unsafe.

## What is rebase?

Rebase takes commits from one line of development and replays them onto another base commit. This can make the resulting history appear more linear, but it creates new commit IDs for the replayed commits because the parent relationship changes.

For example, if `feature/search-filter` was created from an older `main`, you may rebase the feature branch onto an updated `main` to replay the feature commits on top of the latest base.

## Basic rebase example

First make sure your work is saved and your working tree is in an appropriate state:

```bash
git status
git switch main
git fetch origin
git log --oneline --graph --decorate --all
```

If the latest base is available locally as `origin/main`, rebase the feature branch onto it:

```bash
git switch feature/search-filter
git rebase origin/main
```

This example assumes the remote is named `origin` and its base branch is named `main`. Adjust the names to match your repository. `git fetch` downloads remote data but does not itself integrate it into your current branch.

A local-only alternative is `git rebase main`, provided your local `main` is the base you intend to use.

## Rebase conflicts

If a conflict occurs, Git pauses and marks the files that need attention:

```bash
git status
```

Edit each conflicted file, remove the conflict markers, and keep the correct final content. Then stage the resolution and continue:

```bash
git add path/to/resolved-file
git rebase --continue
```

Repeat as needed. To stop and return to the state from before the rebase, use:

```bash
git rebase --abort
```

After the operation, inspect `git status` and the resulting history. Do not continue while conflict markers or unresolved files remain.

## Important: do not casually rebase shared history

**Rule of thumb:** rebasing commits that exist only on your local feature branch is usually manageable. Rebasing commits already shared with teammates changes their IDs and may force collaborators to repair their history. Do not rebase published/shared commits unless your team explicitly agrees on the procedure.

If you rebased a branch that was already pushed, do not immediately force-push. Coordinate with collaborators first. If the team confirms that updating the remote branch is appropriate, prefer `git push --force-with-lease` over plain `--force`; it offers a safeguard against overwriting remote changes you have not fetched. It is still a history-rewriting operation and is not risk-free.

## Merge or rebase?

| Choose merge when… | Choose rebase when… |
|---|---|
| You want to preserve the actual integration point in history. | You are updating private/local commits on top of a newer base. |
| The team prefers merge commits for collaboration. | A linear history is useful and the team has agreed to the workflow. |
| The source branch has already been shared and history should not be rewritten. | The commits being replayed are not yet shared, or rewriting is explicitly coordinated. |

## Common mistakes

- Rebasing while working-tree changes are not safely accounted for.
- Treating rebase as a command that merely “merges without a commit.” It rewrites/replays commits and creates new IDs.
- Forgetting to use `git rebase --abort` when you decide not to continue.
- Force-pushing a rewritten branch without checking whether others have pushed to it.

## Summary

Rebase replays commits on a new base and can create a linear history. Use it deliberately, resolve conflicts one commit at a time, and avoid rewriting history other people already depend on.

## Further reading

- [Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
- [git-rebase](https://git-scm.com/docs/git-rebase)
