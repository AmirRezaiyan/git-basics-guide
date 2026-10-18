# Beginner Exercises

These exercises reinforce the core workflow. Work inside a disposable practice repository, not inside a project containing important work.

## Setup

```bash
mkdir git-guide-practice
cd git-guide-practice
git init
git config user.name "Practice User"
git config user.email "practice@example.com"
```

The identity commands apply only to this repository because they omit `--global`. You can use a different name and email for practice.

## Exercise 1: Make the first commit

**Tasks**

1. Create `notes.txt` with the line `Learning Git`.
2. Run `git status`.
3. Stage the file and review the staged diff.
4. Commit it with the message `Add learning notes`.
5. Confirm the working tree is clean.

**Hints**

Use `git add notes.txt`, `git diff --staged`, and `git commit -m "Add learning notes"`.

<details>
<summary>Solution</summary>

```bash
echo "Learning Git" > notes.txt
git status
git add notes.txt
git diff --staged
git commit -m "Add learning notes"
git status
```

</details>

## Exercise 2: Stage only the intended change

**Tasks**

1. Append a second line to `notes.txt`.
2. Create a new file named `todo.txt`.
3. Stage only `notes.txt`.
4. Use `git status` to confirm `todo.txt` is still untracked.
5. Commit the staged change.

<details>
<summary>Solution</summary>

```bash
echo "Understand staging" >> notes.txt
echo "Practise branches" > todo.txt
git status
git add notes.txt
git diff --staged
git status
git commit -m "Add staging note"
```

</details>

## Exercise 3: Compare working and staged changes

**Tasks**

1. Modify `notes.txt` and stage it.
2. Modify it again without staging the second edit.
3. Compare `git diff` with `git diff --staged`.
4. Explain in your own words why the outputs differ.

<details>
<summary>Solution outline</summary>

`git diff` shows unstaged changes between the working tree and index. `git diff --staged` shows staged changes between the index and `HEAD`. Re-stage the file only after reviewing both sets of changes.

</details>

## Exercise 4: Create a branch

**Tasks**

1. Create and switch to `docs/first-change`.
2. Add a file named `branch-notes.txt`.
3. Commit it.
4. Switch back to the original branch and confirm the file is not present in that branch's working tree when no other changes interfere.
5. Inspect all branches.

<details>
<summary>Solution</summary>

```bash
git switch -c docs/first-change
echo "This file belongs to a branch" > branch-notes.txt
git add branch-notes.txt
git commit -m "Add branch notes"
git switch main
git branch --all
```

If your initial branch is not named `main`, use the name shown by `git branch` instead.

</details>

## Exercise 5: Use `.gitignore`

**Tasks**

1. Create a file named `.gitignore`.
2. Add `local.tmp` to it.
3. Create `local.tmp` and run `git status`.
4. Explain why a newly created file that matches `.gitignore` does not appear as an ordinary untracked file.

<details>
<summary>Solution</summary>

```bash
echo "local.tmp" > .gitignore
echo "temporary data" > local.tmp
git status
```

This works for an untracked file. If the file was already tracked, adding it to `.gitignore` alone does not stop Git from tracking it.

</details>

## Self-check

You are ready for the intermediate exercises when you can explain:

- The difference between the working tree, index, and `HEAD`.
- Why reviewing `git diff --staged` before a commit matters.
- Why a branch is not the same as a second project directory.
- Why `.gitignore` does not remove a file that is already tracked.
