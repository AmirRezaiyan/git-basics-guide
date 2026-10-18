# 9. Remote Repositories and Collaboration

## Learning objectives

Understand remotes and remote-tracking branches, add or inspect a remote, fetch updates, integrate remote work, and push local commits.

## What is a remote?

A remote is a named reference to another repository location. It may be hosted on GitHub, another Git service, or a server on a network. `origin` is the conventional name Git assigns to the repository you cloned; it is not a special requirement.

## Inspect remotes

```bash
git remote
git remote -v
git remote show origin
```

`-v` displays fetch and push URLs. Before sharing terminal output, check that it does not expose private infrastructure details or credentials embedded in an improperly configured URL.

## Clone an existing repository

```bash
git clone https://github.com/OWNER/REPOSITORY.git
cd REPOSITORY
git remote -v
```

Replace the URL with the real repository URL. Cloning normally configures `origin` and creates a local branch that tracks the remote's default branch.

## Add a remote to an existing local repository

If a local repository does not already have a remote:

```bash
git remote add origin https://github.com/OWNER/REPOSITORY.git
git remote -v
```

Do not run `git remote add origin` if `origin` already exists. Inspect `git remote -v` first. If the URL is wrong, update it with `git remote set-url origin <url>` after confirming the intended destination.

## Fetch versus pull

Fetch downloads objects and updates remote-tracking references; it does not by itself merge the downloaded work into your current branch.

```bash
git fetch origin
git branch --all
git log --oneline --graph --decorate --all
```

Pull fetches remote updates and then integrates them into the current branch according to the chosen configuration/options. For a simple fast-forward-only update:

```bash
git pull --ff-only
```

This succeeds only when Git can advance the current branch without creating a merge commit. If it fails because the branches have diverged, stop and inspect the situation; do not blindly reset your branch to the remote.

## Push a branch

```bash
git switch -c docs/improve-guide
# Make a change, then:
git add README.md
git commit -m "Improve guide introduction"
git push -u origin docs/improve-guide
```

The `-u` option sets the upstream tracking relationship for the branch, making later plain `git push` and `git pull` easier when appropriate. The branch must be committed before it can be pushed.

## Track an existing remote branch

After fetching, you may create a local tracking branch for a remote branch:

```bash
git fetch origin
git switch --track origin/feature/search-filter
```

This assumes that `origin/feature/search-filter` exists and has not already been checked out as a local branch. If Git cannot infer or find the branch, inspect `git branch --all` and follow Git's message.

## Understand divergence

Check status and graph before integrating work:

```bash
git status
git fetch origin
git log --oneline --graph --decorate --all
```

The remote-tracking reference `origin/main` is your local record of the last fetched state of the remote's `main` branch; it is not a live network view. Fetch before relying on it for the latest remote state.

## Typical GitHub workflow

1. Clone the repository.
2. Create a feature branch.
3. Make a focused set of changes and commit them.
4. Push the feature branch.
5. Open a pull request on the hosting service when review is appropriate.
6. After the branch is merged, update your local base branch and remove the feature branch when safe.

Git itself supports branching and remotes. Pull requests are a feature of hosting platforms such as GitHub, not a Git command.

## Common mistakes

- **Pushing the wrong branch:** confirm `git status` and `git branch --show-current`.
- **Assuming `origin` always exists:** inspect with `git remote -v`.
- **Confusing fetch with pull:** fetch downloads and updates references; pull also integrates changes.
- **Using `--force` to fix a rejected push:** understand why the push was rejected before changing remote history.
- **Embedding credentials in a URL:** use supported HTTPS authentication or SSH keys and never commit tokens.

## Summary

Inspect remotes, fetch before making decisions about remote history, integrate changes deliberately, and push committed branches. In shared projects, use pull requests and coordinate any history-rewriting operation.

## Further reading

- [Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)
- [git-remote](https://git-scm.com/docs/git-remote)
- [git-fetch](https://git-scm.com/docs/git-fetch)
- [git-pull](https://git-scm.com/docs/git-pull)
- [git-push](https://git-scm.com/docs/git-push)
