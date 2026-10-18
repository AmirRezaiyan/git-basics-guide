# 3. Commits and History

## Learning objectives

Create useful commits, inspect history, compare files between revisions, and find the commit that introduced a change.

## What is a commit?

A commit is a recorded snapshot of the repository at a point in time, along with metadata such as its author, timestamp, message, and parent commit(s). Git identifies commits with object IDs (often shown as abbreviated hashes). A branch is a movable reference to a commit; it is not a separate copy of every file.

## Write clear commit messages

A commit message should tell a future reader what changed. Examples:

```text
Add examples for the staging area
Fix the rebase conflict instructions
Document remote-tracking branches
```

Avoid messages such as `stuff`, `changes`, or `final`. One useful convention is a short imperative subject line (for example, `Add...`, `Fix...`, `Document...`). A team may use a more specific convention, such as Conventional Commits; follow the project's existing standard if one exists.

```bash
git add docs/02-basic-workflow.md
git commit -m "Explain the staging area"
```

## Inspect history

```bash
git log
git log --oneline
git log --oneline --graph --decorate --all
git show HEAD
git show <commit-id>
```

- `git log` displays commits reachable from the current history by default.
- `--oneline` condenses each commit to one line.
- `--graph` draws an ASCII representation of branching and merges.
- `--decorate` displays labels such as branches and tags.
- `--all` includes histories reachable from all refs, including other local branches and fetched remote-tracking branches.
- `git show` displays details for a commit, including its patch by default.

Replace `<commit-id>` with a real commit hash from your repository. Angle-bracket placeholders are not literal command arguments.

## Compare changes

```bash
git diff                    # working tree against the index
git diff --staged           # index against HEAD
git diff HEAD                # working tree and index changes against HEAD
git diff <commit-a> <commit-b>
git diff <commit-id> -- path/to/file
```

The last example limits output to a particular path. To compare a file with a previous version, use `git diff <commit-id> HEAD -- path/to/file` or inspect a revision with `git show <commit-id>:path/to/file`.

## Find who changed a line

```bash
git blame path/to/file
```

`git blame` annotates lines with the commit and author associated with the last change to those lines. It is a navigation aid, not proof of responsibility or intent; lines may have moved, been copied, or been changed by formatting.

## Search commit messages

```bash
git log --oneline --grep="rebase"
git log -- path/to/file
```

The first searches commit messages (case sensitivity depends on options). The second shows commits that affected a path. For advanced searches, see `git log --help`.

## Amend a commit (use carefully)

If you have just made a local commit and need to correct its message or include an omitted change, you can amend it:

```bash
git add forgotten-file.md
git commit --amend
```

Amending replaces the current commit with a new commit. It changes the commit ID. Avoid amending commits that other people have already based work on or that have been shared, unless the team explicitly coordinates the history rewrite.

To change only the message, use `git commit --amend -m "Correct commit message"`. Check `git status` and the commit content before amending.

## Summary

Commits make project history reviewable. Use `git log` to navigate history, `git show` to inspect a commit, `git diff` to compare states, and `git blame` to investigate line history without treating it as a complete explanation of intent.

## Further reading

- [Viewing the Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
- [git-log](https://git-scm.com/docs/git-log)
- [git-show](https://git-scm.com/docs/git-show)
- [git-blame](https://git-scm.com/docs/git-blame)
