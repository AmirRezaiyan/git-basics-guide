# 10. Resolving Git Conflicts

## Learning Objectives

By the end of this chapter, you should be able to:

- Understand why Git reports a merge conflict.
- Recognize conflict markers and identify the conflicting sections.
- Resolve conflicts without accidentally losing important changes.
- Complete or abort a merge safely.
- Resolve conflicts during a rebase.
- Verify that no unresolved conflicts remain before completing an operation.

## What Is a Git Conflict?

A conflict occurs when Git cannot automatically combine changes from different branches or commits.

For example, two branches may modify the same lines in a file. Git can often combine changes to different parts of a file automatically, but it cannot always determine which version should be kept when changes overlap.

A conflict is not necessarily a problem with the repository. It means Git needs you to decide how the changes should be combined.

Common causes include:

- Two branches modify the same lines of a file.
- One branch modifies a file while another deletes it.
- Changes to the same file cannot be combined automatically.
- A rebase attempts to replay a commit whose changes conflict with the target branch.

## Recognize Conflict Markers

A conflicted text file may contain markers that separate the competing versions.

For example, the following HTML displays a simplified conflict-marker example:

<pre>
&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD
Content from the current side of the operation
&#61;&#61;&#61;&#61;&#61;&#61;&#61;
Content from the other side of the operation
&gt;&gt;&gt;&gt;&gt;&gt;&gt; feature/example
</pre>

The markers have these meanings:

- `<<<<<<< HEAD` begins the first version of the conflicting section.
- `=======` separates the two versions.
- `>>>>>>> feature/example` ends the conflicting section and identifies the other side of the operation.

The labels depend on the operation and context. During a merge, `HEAD` generally refers to the version from the currently checked-out branch. During a rebase, the interpretation of the current and incoming sides requires extra care.

The example above is illustrative. In a real conflicted file, the markers appear as literal text that you must resolve.

## Start by Inspecting the Repository

Before editing any conflicted file, inspect the current state:

```bash
git status
```

Git lists files that have conflicts. They may be described as unmerged paths or shown with statuses such as `UU`, depending on the type of conflict.

To list files that still have unresolved conflicts, run:

```bash
git diff --name-only --diff-filter=U
```

To inspect the changes in the working tree, run:

```bash
git diff
```

For an individual conflicted file, you can also inspect its diff:

```bash
git diff -- path/to/file
```

Replace `path/to/file` with the actual file path.

Do not start by deleting conflict markers blindly. First determine what each side changed and what the final content should accomplish.

## 1. Resolve a Merge Conflict

Suppose you have a branch named `feature/example` and want to merge it into `main`.

Start by checking out the target branch and attempting the merge:

```bash
git switch main
git merge feature/example
```

If Git cannot merge the changes automatically, it reports a conflict and pauses the merge.

### Step 1: Identify the conflicting files

Run:

```bash
git status
```

Read the output and identify every file reported as unmerged.

### Step 2: Open each conflicted file

Open the files in your editor.

Find the conflict markers and read the content on both sides. One side represents the current branch's content; the other represents the changes being merged.

Do not assume that one entire side is always correct. Sometimes you need the first version, sometimes the second, and sometimes a combination of both.

### Step 3: Resolve the content

Edit each conflicted section to produce the final intended content.

For example, imagine a configuration file has conflicting values for a timeout. You should decide which value is appropriate for the application rather than blindly keeping one side.

After resolving a section:

- Keep the content you actually need.
- Remove all conflict markers from the resolved section.
- Check indentation, syntax, and surrounding content.
- Ensure that neither side's necessary changes have been lost.

Save the file.

### Step 4: Stage each resolved file

After resolving a file, mark it as resolved:

```bash
git add path/to/file
```

Repeat this for every file you have resolved.

Staging a file tells Git that you have resolved its conflict. It does not automatically verify that the resulting content is logically correct.

### Step 5: Verify that conflicts are resolved

Run:

```bash
git status
git diff --name-only --diff-filter=U
git diff --cached
```

The unresolved-file command should produce no output if every conflict has been resolved.

Review the staged changes carefully. Confirm that the final content is correct and that no accidental changes were introduced.

### Step 6: Complete the merge

When every conflict is resolved and staged, complete the merge:

```bash
git commit
```

Git may open an editor containing a suggested merge-commit message. Save and close the editor to complete the commit.

Depending on the repository and the operation, Git may provide other completion instructions. Follow the instructions displayed by Git.

Finally, inspect the result:

```bash
git status
git log --oneline --graph -5
```

Confirm that the merge completed and that the working tree is in the state you expect.

## 2. Abort a Merge Safely

If you decide that you do not want to continue a merge that is currently in progress, you can normally abort it with:

```bash
git merge --abort
```

This attempts to return the repository to the state it had before the merge began.

**Important:** If you had uncommitted changes before starting the merge, recovery may be more complicated. Git specifically warns that aborting may not always reconstruct those changes perfectly in unusual situations.

For that reason, inspect your working tree before beginning a merge and avoid starting one with unrelated, uncommitted changes.

After aborting, verify the result:

```bash
git status
```

Do not run `git merge --abort` when no merge is in progress.

## 3. Resolve Conflicts During a Rebase

A rebase applies commits from one branch on top of another base. A conflict may occur while Git is replaying one of those commits.

For example:

```bash
git switch feature/example
git rebase main
```

If a conflict occurs, Git pauses and asks you to resolve it.

### Step 1: Inspect the conflict

Run:

```bash
git status
git diff --name-only --diff-filter=U
```

Identify the files that need attention.

### Step 2: Edit and resolve the files

Open each conflicted file and resolve the conflicting content, just as you would during a merge.

During a rebase, the terms `ours` and `theirs` can be counterintuitive:

- `ours` generally refers to the new base and the commits already applied to it.
- `theirs` generally refers to the commit currently being replayed.

Do not choose a side based on its label alone. Inspect the actual content and decide which result is correct.

### Step 3: Stage the resolved files

For every file you resolve, run:

```bash
git add path/to/file
```

Replace the placeholder with the actual path.

### Step 4: Continue the rebase

After resolving and staging all conflicts, continue:

```bash
git rebase --continue
```

Git may apply the next commit and encounter another conflict. If that happens, repeat the same inspection and resolution process.

After the rebase finishes, inspect the result:

```bash
git status
git log --oneline --graph -10
```

A rebase rewrites commit history by creating new commits. If the branch was already published, review the collaboration implications before force-pushing any rewritten history.

### Abort a rebase

If you decide not to continue a rebase that is in progress, run:

```bash
git rebase --abort
```

This attempts to restore the branch to its state before the rebase began.

Use this command only when a rebase is in progress. Always inspect the repository afterward.

### Skip a commit during a rebase

Git also provides:

```bash
git rebase --skip
```

This skips the commit currently being replayed.

**Warning:** Skipping a commit can discard the changes introduced by that commit from the rebased result. Use it only when you have deliberately decided that the commit should not be applied.

Do not use `--skip` as a routine method for resolving conflicts.

## 4. Handle File Deletion Conflicts

Not every conflict involves two different versions of the same line.

A conflict can occur when one branch deletes a file while another branch modifies it.

In this situation, inspect the file and the history to determine whether the file should remain or be deleted.

For example:

```bash
git status
git log --oneline -- path/to/file
```

Possible outcomes include:

- Keep the modified file if its content is still required.
- Delete the file if the deletion was intentional.
- Reconstruct a suitable version if the file needs to remain but its contents require further changes.

If you decide to keep the file, edit it as necessary and stage it:

```bash
git add path/to/file
```

If you decide to keep the deletion, stage the deletion:

```bash
git rm path/to/file
```

Replace the path with the actual conflicted file.

Then verify the repository status and ensure that the conflict is no longer listed as unresolved.

## 5. Conflict Prevention

You cannot prevent every conflict, but a careful workflow can reduce their frequency.

### Keep branches up to date

Before starting work, inspect the current branch and working tree:

```bash
git status
git branch --show-current
git fetch origin
```

Fetching downloads information from the remote without automatically integrating it into your current branch.

Review the project's workflow before deciding whether to merge or rebase remote changes into your branch.

### Make focused commits

Small, focused commits are generally easier to understand and review than large commits containing unrelated changes.

### Coordinate shared files

If several people are editing the same files, communicate about changes to shared sections, configuration files, or public interfaces.

### Integrate changes regularly

Integrating relevant changes at suitable intervals can reduce the amount of divergence between branches. Follow your team's merge or rebase policy.

### Review changes before integrating

Use:

```bash
git diff
git diff --staged
git log --oneline --graph --decorate --all
```

These commands help you understand your working changes, staged content, and commit history before integrating work.

## Quick Decision Guide

| Situation | Command | Purpose |
|---|---|---|
| Inspect repository status | `git status` | Identify conflicted files and the current operation |
| List unresolved conflicts | `git diff --name-only --diff-filter=U` | Find files that still require resolution |
| Inspect unstaged changes | `git diff` | Review working-tree modifications |
| Stage a resolved file | `git add <file>` | Mark the file as resolved |
| Stage a resolved deletion | `git rm <file>` | Record the file deletion and resolve that path |
| Complete a merge | `git commit` | Finish the merge after resolving conflicts |
| Abort an in-progress merge | `git merge --abort` | Attempt to return to the pre-merge state |
| Continue a rebase | `git rebase --continue` | Resume replaying commits after resolving conflicts |
| Abort an in-progress rebase | `git rebase --abort` | Attempt to return to the pre-rebase state |
| Skip the current rebase commit | `git rebase --skip` | Omit the current commit from the rebase |

## Common Mistakes

### Removing markers without understanding the changes

Deleting the markers alone is not sufficient. You must also choose the correct final content.

### Staging before resolving the content

Staging a file does not prove that its content is correct. Review it after editing and before completing the operation.

### Assuming `git add` completes a merge

`git add` marks a conflicted file as resolved. You still need to complete the operation using the appropriate merge or rebase command.

### Using `git reset --hard` as a conflict-resolution shortcut

This command can discard working-tree changes and move the current branch reference. It is not a safe general-purpose conflict-resolution tool.

### Force-pushing rewritten history without coordination

Rebasing published commits can affect other contributors. Follow the team's history-rewriting and push policies.

## Practice Exercise

Use a disposable repository so you do not risk important work.

### Exercise 1: Create a Merge Conflict

1. Create a new repository and make an initial commit containing a text file.
2. Create a branch named `feature/example`.
3. Change a particular line on the feature branch and commit the change.
4. Switch back to the main branch.
5. Change the same line differently and commit the change.
6. Attempt to merge the feature branch.
7. Inspect the conflict with `git status` and `git diff`.

### Exercise 2: Resolve the Conflict

1. Open the conflicted file.
2. Decide what the final content should be.
3. Remove the conflict markers from the final version.
4. Stage the resolved file.
5. Confirm that `git diff --name-only --diff-filter=U` produces no output.
6. Complete the merge and inspect the result with `git log --oneline --graph`.

### Exercise 3: Practise Rebase Recovery

1. Create a disposable repository with a main branch and a feature branch.
2. Make conflicting changes on both branches and commit them.
3. Start a rebase that produces a conflict.
4. Inspect the affected files and resolve the conflict.
5. Stage the resolved files and run `git rebase --continue`.
6. Repeat if another conflict appears.
7. Inspect the final history and repository status.

Do not practise destructive recovery commands in a repository containing important uncommitted work.

## Key Takeaway

Git conflicts require a deliberate decision about how changes should be combined.

Inspect the repository, understand both sides of the conflict, create the intended final content, stage every resolved file, and verify the result before completing the merge or rebase.

When you are uncertain, stop and inspect the repository state rather than using destructive commands as a shortcut.