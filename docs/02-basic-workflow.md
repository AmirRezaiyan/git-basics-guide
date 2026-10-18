# 2. The Basic Git Workflow

## Learning objectives

Learn how the working tree, staging area, and repository relate to one another; stage and commit selected changes; inspect a repository's state; and prevent unwanted files from being tracked.

## The three places to understand

- **Working tree:** the files and directories you see and edit.
- **Staging area (index):** the snapshot of changes prepared for the next commit.
- **Repository history:** the commits already recorded by Git.

A common workflow is:

```text
Edit files -> inspect changes -> stage selected changes -> inspect staged changes -> commit
```

A commit records the staged snapshot, not every edit that happens to exist in the working tree.

## Inspect the current state

```bash
git status
git status --short
```

The full form explains whether files are untracked, modified, staged, or whether the branch is ahead/behind its upstream. The short form provides a compact status view. Run `git status` frequently; it is one of the safest and most useful Git commands.

## Stage and commit changes

```bash
git add README.md
git status
git diff --staged
git commit -m "Improve project documentation"
```

`git add` stages the file's current content. If you edit the file again after staging it, the later edits are not automatically included in the staged snapshot; stage again when ready.

Stage all changes under the current directory with:

```bash
git add .
```

Use this only after reviewing `git status` and understanding which files may be included. For sensitive or mixed-purpose changes, stage explicit paths rather than blindly staging everything.

To stage tracked and untracked changes across the repository, `git add -A` is another option. It also stages removals. Check the staged result before committing.

## Understand `git diff`

```bash
git diff                 # unstaged changes
git diff --staged        # staged changes, ready for the next commit
git diff HEAD            # tracked changes relative to HEAD, including staged and unstaged edits
```

Diff output commonly uses `-` for removed lines and `+` for added lines. Read the surrounding context; a line appearing as removed and added can simply mean it changed.

## Track file changes

To stage deletion of a tracked file:

```bash
git rm old-file.txt
git status
git commit -m "Remove obsolete file"
```

To stop tracking a file while keeping it on disk, first add it to `.gitignore`, then remove it from the index:

```bash
git rm --cached local-settings.json
git status
```

This removes the file from Git's index but leaves the working copy. If the file contains credentials and was already committed or pushed, ignoring it does not erase it from history; rotate the exposed credential and follow the hosting provider's remediation guidance.

## Use `.gitignore`

`.gitignore` tells Git which untracked files to ignore. It does not automatically untrack files that are already committed.

Example:

```gitignore
# Environment-specific files
.env
.env.*
!.env.example

# Python
__pycache__/
*.py[cod]

# Node
node_modules/
dist/

# Editor and OS files
.DS_Store
Thumbs.db
```

This is only an example; adapt it to your project's actual stack. Never commit secrets merely because they are absent from the latest diff. Check `git status --ignored` if you need to inspect ignored paths.

## A complete small workflow

```bash
git status
git diff
# Edit your file, then:
git add path/to/file.md
git diff --staged
git commit -m "Explain the new workflow"
git status
```

Expected result after a successful commit: `git status` reports a clean working tree unless additional changes remain.

## Common mistakes

- **Committing before reviewing:** inspect `git diff --staged` before each commit.
- **Expecting `git add` to commit:** staging and committing are separate steps.
- **Assuming `.gitignore` removes an already tracked file:** it does not; use `git rm --cached` when appropriate.
- **Using vague commit messages:** say what changed, not just “update” or “fix”.

## Summary

Inspect first, stage intentionally, review the staged diff, and then commit. This loop makes history understandable and reduces accidental inclusion of unrelated files.

## Further reading

- [Recording Changes to the Repository](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)
- [git-status](https://git-scm.com/docs/git-status)
- [git-add](https://git-scm.com/docs/git-add)
- [gitignore](https://git-scm.com/docs/gitignore)
