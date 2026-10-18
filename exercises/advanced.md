# Advanced Exercises

These exercises involve history inspection and rewriting. Use a disposable repository and read the linked chapters first. Never practise destructive commands in a repository containing work you need.

## Exercise 1: Recover a commit using reflog

**Tasks**

1. Create two commits on a local branch.
2. Record the latest commit ID.
3. Move a branch reference in a controlled practice scenario (do not use valuable work).
4. Run `git reflog` and locate the earlier commit.
5. Create a new recovery branch at that commit and inspect it.

**Expected concepts**

The reflog records recent local reference movements. Creating a recovery branch allows you to inspect a candidate commit without immediately resetting your main branch. Reflogs can expire and are not a backup system.

## Exercise 2: Cherry-pick a fix

**Tasks**

1. Create a branch and commit a small, isolated change.
2. Switch to a different branch.
3. Use `git cherry-pick <commit-id>` to apply that change.
4. Compare the resulting commit IDs and history.
5. Explain why cherry-pick creates a new commit rather than moving the original commit.

**Expected concepts**

Cherry-pick applies the selected commit's changes to the current branch. The new commit usually has a different ID because its parent and context differ.

## Exercise 3: Bisect a known regression

**Tasks**

1. Create a short linear history with at least six commits.
2. Introduce a simple, reproducible text-based “bug” in one commit.
3. Identify a known-good earlier commit and the current bad commit.
4. Use `git bisect start`, mark the current commit bad, then mark the known-good commit good.
5. At each candidate commit, check the file and mark the candidate good or bad.
6. Finish with `git bisect reset` and explain how binary search reduces the number of commits to test.

**Expected concepts**

Bisect narrows the range between a known-good and known-bad commit. Correct results depend on accurate classification at each step. Use `git bisect reset` to leave bisect mode and return to the original branch state.

## Exercise 4: Interactive rebase on private commits

**Tasks**

1. Create a feature branch with three small, local-only commits.
2. Run `git rebase -i <base-commit>` where the base is the commit just before those three commits.
3. Reword one commit message or squash two logically related commits.
4. Inspect the resulting history.
5. Explain why you should not casually do the same thing to a branch other people have already based work on.

**Expected concepts**

Interactive rebase can edit the commit sequence and creates new commit IDs. Rewriting shared history affects collaborators and must be coordinated.

## Exercise 5: Explain a force-push decision

**Scenario**

A feature branch was pushed, then rebased locally. A normal push is rejected because the remote branch no longer matches the rewritten local history.

**Tasks**

Write a safe checklist before changing the remote branch. Include fetching, inspecting local and remote history, checking whether anyone else has pushed commits, and coordinating with the team.

**Expected concepts**

Do not use `--force` as a reflex. When a team explicitly agrees to replace the remote branch, `--force-with-lease` offers a check against overwriting remote updates that have not been fetched. It is still potentially destructive and is not automatically safe in every workflow.

## Completion checklist

- [ ] I can read a commit graph.
- [ ] I can explain branch references and `HEAD`.
- [ ] I can distinguish history-preserving fixes from history rewriting.
- [ ] I understand that reflog is local and temporary.
- [ ] I know when to stop and seek help rather than guess at a destructive operation.
