# 12. Advanced Git Tools

This chapter introduces several powerful tools. Practise them in a disposable repository first, especially when a command changes commit history.

## 1. Cherry-pick a commit

`git cherry-pick` applies the changes introduced by one or more existing commits to the current branch by creating new commit(s).

```bash
git status
git log --oneline --all
git switch target-branch
git cherry-pick <commit-id>
```

Replace `<commit-id>` with the actual commit you want to copy. The target branch receives a new commit; the original commit is not moved. Cherry-pick can conflict. Resolve the files, stage them, and continue with `git cherry-pick --continue`; use `git cherry-pick --abort` to stop an in-progress cherry-pick.

Cherry-pick is useful for selectively applying a fix, but avoid using it as a substitute for understanding the branch relationship. Repeated cherry-picks can create duplicate changes across histories.

## 2. Use reflog to inspect recent local reference movements

```bash
git reflog
```

The reflog records recent updates to local references such as `HEAD`. It can help find a commit after a reset or branch movement. Reflogs are local and expire; they are not a remote backup. See [Undoing Changes](07-undoing-changes.md) before attempting recovery.

## 3. Find a regression with bisect

`git bisect` performs a binary search through commit history to help identify the commit that introduced a bug. You need a way to identify a known-good state and a known-bad state.

```bash
git bisect start
git bisect bad                 # current commit is known to be bad
git bisect good <known-good-commit>
```

Git checks out a candidate commit. Run your tests or reproduce the bug, then tell Git whether that candidate is good or bad:

```bash
git bisect good
# or:
git bisect bad
```

Continue until Git identifies a likely first bad commit. Then finish the session:

```bash
git bisect reset
```

Use the real result from your investigation; the example deliberately does not invent a commit ID. If the result cannot be classified reliably, read `git bisect --help` for skip options and other details.

## 4. Interactive rebase

Interactive rebase lets you review and edit a sequence of commits—for example, reword messages, reorder commits, combine related commits, or remove an accidental local commit.

```bash
git switch feature/my-work
git status
git rebase -i <base-commit>
```

Choose the correct base: Git offers the commits after that base for editing. Read the todo list before saving it and understand each action before using it.

**Do not casually use interactive rebase on commits that have been shared with others.** It can change commit IDs and require coordinated updates. If conflicts occur, resolve them and use `git rebase --continue`, or return to the pre-rebase state with `git rebase --abort`.

## 5. Inspect reachability and references

```bash
git branch --contains <commit-id>
git tag --contains <commit-id>
git log --oneline --graph --decorate --all
```

These commands help answer whether a commit is reachable from a branch or tag and how refs relate to the commit graph. Substitute a real commit ID.

## 6. Useful aliases

Aliases can shorten frequent commands:

```bash
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Then run `git st` or `git lg`. Aliases are optional configuration, not built-in commands, and they do not replace understanding the underlying operation.

## Advanced command safety checklist

Before changing history or moving refs:

1. Run `git status` and save or back up important changes.
2. Inspect the current branch and relevant history.
3. Confirm whether commits have been pushed or shared.
4. Read the relevant command help if the effect is unclear.
5. Prefer a recovery branch or backup for important work.

## Summary

Cherry-pick applies selected changes, reflog helps investigate local reference history, bisect searches commits for regressions, and interactive rebase edits a commit sequence. All are powerful tools; use them with an understanding of their effect on history.

## Further reading

- [git-cherry-pick](https://git-scm.com/docs/git-cherry-pick)
- [git-reflog](https://git-scm.com/docs/git-reflog)
- [git-bisect](https://git-scm.com/docs/git-bisect)
- [git-rebase](https://git-scm.com/docs/git-rebase)
- [git-config](https://git-scm.com/docs/git-config)
