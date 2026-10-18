# 7. Undoing Changes Safely

## Learning Objectives

By the end of this chapter, you should be able to:

- Identify whether a change is unstaged, staged, or committed.
- Choose an undo command that matches the situation.
- Understand when a command can discard work or rewrite history.
- Recover from some mistakes using Git's reflog.

## Start with Inspection

Before undoing anything, run:

```bash
git status
git diff
git diff --staged
git log --oneline -5
```

Inspect the output before choosing an undo command. Git has different commands for changes that are unstaged, staged, or already committed.

## 1. Discard Unstaged Changes to a File

To discard changes in a tracked file and restore it to the version in the index:

```bash
git restore path/to/file
```

**Warning:** This discards unstaged changes in that file. Make sure you do not need them before running the command.

For example:

```bash
git restore README.md
```

To discard unstaged changes in all tracked files in the current directory and its subdirectories:

```bash
git restore .
```

Use this only when you intentionally want to discard those changes. It does not remove untracked files.

## 2. Unstage a File

If you staged a file with `git add` but have not committed it, you can remove it from the staging area without discarding its working-tree changes:

```bash
git restore --staged path/to/file
```

For example:

```bash
git restore --staged README.md
```

The changes remain in the file; they are simply no longer staged.

To unstage all staged changes:

```bash
git restore --staged .
```

This updates the index but preserves your working-tree changes.

## 3. Undo the Latest Commit Without Losing Its Changes

If the latest commit has not been pushed and you want to undo the commit while keeping its changes staged:

```bash
git reset --soft HEAD~1
```

To undo the latest commit while keeping its changes in the working tree but unstaged:

```bash
git reset HEAD~1
```

The second command uses Git's default mixed reset mode.

Both commands move the current branch pointer to an earlier commit. The `--soft` option preserves the index and working tree, while the default mixed mode resets the index but preserves the working-tree changes.

**Warning:** `git reset` can rewrite the branch's visible history. Be especially careful when commits have already been pushed or shared with others.

## 4. Reverse a Commit with `git revert`

If a commit has already been shared with others, `git revert` is generally the safer choice for reversing its changes:

```bash
git revert <commit-hash>
```

This creates a new commit that reverses the changes introduced by the specified commit. It preserves the existing commit history.

Find a commit hash with:

```bash
git log --oneline
```

Replace `<commit-hash>` with the actual commit hash.

For example:

```bash
git revert a1b2c3d
```

The hash above is only an example. Use a real commit hash from your repository.

If Git reports conflicts during a revert, resolve them, stage the resolved files, and continue the revert:

```bash
git add <resolved-file>
git revert --continue
```

To cancel an in-progress revert:

```bash
git revert --abort
```

Use `git revert --abort` only while a revert operation is in progress.

## 5. Recover a Previous Branch Position with `git reflog`

The reflog records recent movements of local references, such as `HEAD`:

```bash
git reflog
```

You may see entries for commits, resets, checkouts, or rebases. If you accidentally moved a branch, identify the relevant earlier commit before attempting recovery.

A cautious recovery workflow is:

```bash
git reflog
git branch recovery-branch <commit-hash>
```

Replace `<commit-hash>` with the actual commit hash shown in the reflog.

This creates a new branch pointing to that commit so you can inspect the recovered state before changing your current branch.

For example:

```bash
git branch recovery-branch HEAD@{1}
```

This is an example only. Inspect your reflog first and make sure the selected entry points to the state you want to recover.

Reflog entries are local and may eventually expire. They are not a substitute for backups.

## 6. Remove Untracked Files Safely

Untracked files are files Git is not currently tracking. They may include new source files that have not yet been added with `git add`.

To preview which untracked files Git would remove:

```bash
git clean -n
```

To preview untracked files and directories:

```bash
git clean -nd
```

Review the output carefully before deleting anything.

To remove untracked files and directories:

```bash
git clean -fd
```

**Warning:** `git clean -fd` can permanently delete untracked work. Preview first, and never use it blindly.

The `-n` option performs a dry run. It shows what would be removed without deleting anything.

## 7. Restore a File from a Commit

If you want to restore a file to the version contained in a particular commit, use:

```bash
git restore --source=<commit-hash> -- path/to/file
```

For example:

```bash
git restore --source=HEAD~1 -- README.md
```

This restores the file's contents from the previous commit into your working tree. It does not automatically create a commit.

Inspect the result before staging it:

```bash
git diff -- README.md
```

If the restored version is what you intended, stage and commit it:

```bash
git add README.md
git commit -m "Restore README to previous version"
```

**Warning:** Restoring a file can overwrite its current working-tree contents. Check for uncommitted changes before proceeding.

## 8. Understand `HEAD`, `HEAD~1`, and Commit Hashes

Git uses `HEAD` to refer to the currently checked-out commit in a typical branch checkout.

- `HEAD` refers to the current commit.
- `HEAD~1` refers to the first parent of the current commit.
- `HEAD~2` refers to the first parent two generations back.
- A commit hash identifies a specific commit.

You can inspect recent commits with:

```bash
git log --oneline --graph -10
```

For example, if your history looks like this:

```text
a1b2c3d (HEAD -> main) Update documentation
b2c3d4e Fix a bug
c3d4e5f Initial commit
```

Then:

- `HEAD` refers to `a1b2c3d`.
- `HEAD~1` refers to `b2c3d4e`.
- `HEAD~2` refers to `c3d4e5f`.

These references depend on the actual commit history of your repository.

## Quick Decision Guide

| Situation | Command | Effect |
|---|---|---|
| Discard unstaged changes in a tracked file | `git restore <file>` | Discards those file changes |
| Unstage a file | `git restore --staged <file>` | Keeps working-tree changes |
| Undo the latest commit and keep changes staged | `git reset --soft HEAD~1` | Moves branch pointer and preserves staging |
| Undo the latest commit and keep changes unstaged | `git reset HEAD~1` | Moves branch pointer and resets the index |
| Reverse a commit with a new commit | `git revert <hash>` | Preserves existing commit history |
| Inspect recent reference movements | `git reflog` | Shows local reference history |
| Create a recovery branch | `git branch <name> <hash>` | Creates a branch at a selected commit |
| Preview untracked-file cleanup | `git clean -nd` | Does not delete anything |
| Delete untracked files and directories | `git clean -fd` | Deletes selected untracked items |
| Restore a file from an earlier commit | `git restore --source=<hash> -- <file>` | Changes the working-tree copy |

## Safe Habits

1. Run `git status` before undoing changes.
2. Inspect the diff before discarding or reverting work.
3. Prefer `git revert` when reversing commits that have already been shared.
4. Preview `git clean` before deleting untracked files.
5. Create a recovery branch before attempting a risky recovery.
6. Never run destructive commands without understanding their effects.
7. If you are uncertain, stop and inspect the repository state before continuing.

## Practice Exercise

Use a temporary repository or a disposable test repository so you do not accidentally lose important work.

### Exercise 1: Unstaged Changes

1. Modify a tracked file.
2. Run `git status`.
3. Run `git diff`.
4. Use `git restore <file>` to discard the change.
5. Run `git status` again and confirm that the change is gone.

### Exercise 2: Staged Changes

1. Modify a tracked file.
2. Run `git add <file>`.
3. Run `git diff --staged`.
4. Use `git restore --staged <file>` to unstage the file.
5. Run `git status` and confirm that the working-tree changes remain.

### Exercise 3: Reverting a Commit

1. Make a small change and commit it.
2. Run `git log --oneline -5`.
3. Identify the commit you just created.
4. Run `git revert <commit-hash>`.
5. Inspect the new commit and confirm that the change has been reversed.

### Exercise 4: Inspecting the Reflog

1. Run `git reflog`.
2. Identify recent movements of `HEAD`.
3. Choose a previous commit from the reflog.
4. Create a recovery branch pointing to that commit.
5. Inspect the recovery branch without changing your main branch.

### Exercise 5: Previewing Cleanup

1. Create an untracked test file.
2. Run `git clean -nd`.
3. Confirm that Git lists the test file without deleting it.
4. Remove the test file manually, or use `git clean -fd` only after reviewing the preview and confirming that the listed files can safely be deleted.

## Key Takeaway

Choose the undo command based on where the change currently exists: in the working tree, in the staging area, or in commit history.

Inspect first, understand the consequences, and use destructive commands only when you are certain.
