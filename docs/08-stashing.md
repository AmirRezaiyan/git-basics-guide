# 8. Stashing Work

## Learning objectives

Temporarily save work without making a commit, inspect stashes, restore a saved change, and understand what stash does not include by default.

## What is a stash?

A stash records selected local modifications so you can return to a clean working tree and work on something else. It is useful for short interruptions, but it should not replace meaningful commits or a backup of important work.

## Save changes

```bash
git status
git stash push -m "WIP: search filter"
```

Git saves changes to tracked files in the working tree and index by default. Check `git status` afterward to verify what remains.

### Include untracked files

Untracked files are not included by default. To include them:

```bash
git stash push -u -m "WIP: include new files"
```

The `-u` option includes untracked files, but not ignored files. Do not use `--all` casually: it can include ignored files as well, including local configuration and generated output.

## List and inspect stashes

```bash
git stash list
git stash show stash@{0}
git stash show -p stash@{0}
```

`stash@{0}` is the most recent stash at the time of inspection; later stash operations can change the indexes. `-p` shows the patch. Inspect a stash before applying it if you have several similar entries.

## Apply versus pop

```bash
git stash apply stash@{0}
```

`apply` tries to reapply the change and keeps the stash entry. When you have verified that the changes are restored correctly, you can remove the stash separately if no longer needed.

```bash
git stash pop stash@{0}
```

`pop` attempts to apply the changes and remove the stash entry if the application succeeds. If conflicts occur or the apply fails, Git may keep the stash. Check `git status` and `git stash list`; do not assume the stash was removed.

## Delete a stash

```bash
git stash drop stash@{0}
```

This permanently removes that stash entry from the stash list. Verify the right entry first. To delete all stash entries, `git stash clear` is available but is destructive; avoid it unless you are sure none of the saved work is needed.

## Example: change tasks safely

```bash
git status
git stash push -u -m "WIP: unfinished documentation"
git switch main
# Finish the urgent task, commit it, then return:
git switch feature/my-work
git stash list
git stash apply stash@{0}
git status
```

The example assumes `feature/my-work` exists and that switching branches is safe. Resolve any conflict caused by applying the stash, then inspect the files and create a normal commit when the work is ready.

## Common mistakes

- Expecting a default stash to include untracked files.
- Using stash as long-term storage.
- Popping a stash before checking which entry is selected.
- Forgetting that applying a stash can create conflicts.

## Summary

Use `git stash push -m` to label temporary work, `git stash list` and `git stash show -p` to inspect it, and choose `apply` when you want to keep the saved entry or `pop` when you want Git to remove it after a successful application.

## Further reading

- [git-stash](https://git-scm.com/docs/git-stash)
