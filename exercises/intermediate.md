# Intermediate Exercises

Use a practice repository. The exercises assume that you understand staging, commits, and branches.

## Setup

```bash
mkdir git-intermediate-practice
cd git-intermediate-practice
git init
git config user.name "Practice User"
git config user.email "practice@example.com"
echo "Project notes" > README.md
git add README.md
git commit -m "Add initial notes"
```

If the initial branch name is not `main`, use its actual name in the steps below.

## Exercise 1: Merge a feature branch

**Tasks**

1. Create `feature/add-purpose`.
2. Add a sentence to `README.md` and commit it on the feature branch.
3. Switch to `main` and merge the feature branch.
4. Review the resulting graph.
5. Delete the feature branch with `git branch -d` after confirming it is merged.

<details>
<summary>Solution</summary>

```bash
git switch -c feature/add-purpose
echo "This project is used to practise Git." >> README.md
git add README.md
git commit -m "Describe practice project"
git switch main
git merge feature/add-purpose
git log --oneline --graph --decorate --all
git branch -d feature/add-purpose
```

A fast-forward merge may occur, so a separate merge commit is not guaranteed.

</details>

## Exercise 2: Revert a commit

**Tasks**

1. Make a commit that adds a sentence you can safely remove.
2. Use `git log --oneline` to find its hash.
3. Run `git revert <commit-id>` with the actual hash.
4. Inspect the new history and confirm that the original commit remains while a new reversal commit is added.

<details>
<summary>Solution outline</summary>

`git revert` records a new commit that reverses the selected commit's changes. It does not erase the original commit from history. This is commonly appropriate for a commit that has already been shared.

</details>

## Exercise 3: Stash unfinished work

**Tasks**

1. Modify a tracked file without committing it.
2. Save the change with a descriptive stash message.
3. Confirm the working tree is clean.
4. List and inspect stashes.
5. Apply the chosen stash and verify the file.

<details>
<summary>Solution</summary>

```bash
git stash push -m "WIP: practice stash"
git status
git stash list
git stash show -p stash@{0}
git stash apply stash@{0}
git status
```

The stash remains after `apply`. Inspect the result before dropping it.

</details>

## Exercise 4: Fetch versus pull

**Tasks**

1. Explain what `git fetch origin` changes locally.
2. Explain why it does not automatically merge remote commits into your current branch.
3. Compare the graph before and after a fetch in a repository with a remote.
4. Explain when `git pull --ff-only` can fail.

**Expected concepts**

Fetch updates objects and remote-tracking references. Pull fetches and integrates the selected remote branch. `--ff-only` refuses to integrate if a fast-forward is not possible, including when local and remote histories have diverged.

## Exercise 5: Resolve a conflict

**Tasks**

1. Create two branches from the same commit.
2. Change the same line of a text file differently on both branches and commit each change.
3. Merge one branch into the other.
4. Inspect the conflict markers, edit the intended final sentence, stage the file, and complete the merge.
5. Verify no conflict markers remain and inspect the graph.

**Hints**

Use `git status` first. Do not select a side blindly; combine the changes if that is the intended result. See [Git Conflicts](../docs/10-git-conflicts.md).

## Self-check

You should be able to distinguish merge from rebase, revert from reset, apply from pop, and fetch from pull. If these distinctions are still unclear, revisit the relevant chapters before using history-rewriting commands in a real project.
