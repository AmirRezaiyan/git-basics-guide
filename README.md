<div align="center">

# Git Basics Guide

**A practical, example-driven guide to Git—from your first commit to confident collaboration.**

[Getting Started](docs/01-getting-started.md) · [Basic Workflow](docs/02-basic-workflow.md) · [Exercises](exercises/beginner.md) · [Glossary](docs/glossary.md)

</div>

---

## About this guide

Git is a distributed version control system used to record changes, inspect project history, experiment safely with branches, and collaborate with other developers. This repository is a structured learning guide with practical commands, explanations, exercises, and safety notes.

The guide is designed for people who are new to Git as well as developers who know the basics but want a more reliable workflow. Git commands are generally the same across operating systems; a few file-creation or navigation examples use Bash/POSIX shell syntax. PowerShell alternatives are noted in the getting-started chapter where relevant.

## What you will learn

- Install Git and configure your identity.
- Understand repositories, commits, the working tree, and the staging area.
- Record changes and inspect differences and history.
- Create, switch, merge, and delete branches safely.
- Understand merge and rebase, including when not to rebase.
- Recover from common mistakes with `restore`, `reset`, `revert`, and `reflog`.
- Temporarily save work with `stash`.
- Work with remotes and collaborate through GitHub or another Git host.
- Resolve merge conflicts and work with tags and selected advanced tools.

## Learning path

| Stage | Chapter | Outcome |
|---|---|---|
| 1. Start | [Getting Started](docs/01-getting-started.md) | Install Git, configure it, and create or clone a repository. |
| 2. Core workflow | [Basic Workflow](docs/02-basic-workflow.md) | Understand status, staging, commits, and `.gitignore`. |
| 3. History | [Commits and History](docs/03-commits-and-history.md) | Review commits and compare versions of files. |
| 4. Branches | [Branching](docs/04-branching.md) | Work on an isolated line of development. |
| 5. Integration | [Merging](docs/05-merging.md) | Integrate a completed branch. |
| 6. History editing | [Rebasing](docs/06-rebasing.md) | Understand linear history and safe rebase practices. |
| 7. Recovery | [Undoing Changes](docs/07-undoing-changes.md) | Choose the correct recovery command for a mistake. |
| 8. Pause work | [Stashing](docs/08-stashing.md) | Temporarily save unfinished changes. |
| 9. Collaboration | [Remote Repositories](docs/09-remote-repositories.md) | Fetch, pull, push, and track remote branches. |
| 10. Troubleshooting | [Git Conflicts](docs/10-git-conflicts.md) | Resolve conflicts during merge or rebase. |
| 11. Releases | [Tags and Releases](docs/11-tags-and-releases.md) | Mark important points in history. |
| 12. Beyond the basics | [Advanced Git](docs/12-advanced-git.md) | Use cherry-pick, reflog, bisect, and interactive rebase carefully. |

For a quick lookup, use the [Glossary](docs/glossary.md). To practise, start with [Beginner Exercises](exercises/beginner.md), then continue to [Intermediate](exercises/intermediate.md) and [Advanced Exercises](exercises/advanced.md).

## Recommended way to practise

Use a disposable repository. Commands such as `git reset --hard`, `git clean`, and some history-rewriting operations can discard work. Do not practise destructive commands in a project that contains important, uncommitted changes.

```bash
mkdir git-practice
cd git-practice
git init
git status
```

Create `notes.txt` in your editor with the text `My first Git practice file`, then stage it and make your first commit:

```bash
git add notes.txt
git commit -m "Add practice notes"
git log --oneline
```

If Git asks you to configure your identity, follow the [first-time setup instructions](docs/01-getting-started.md#first-time-configuration).

## Essential commands at a glance

| Goal | Command |
|---|---|
| Inspect current state | `git status` |
| Stage a file | `git add <file>` |
| Review unstaged changes | `git diff` |
| Review staged changes | `git diff --staged` |
| Commit staged changes | `git commit -m "Describe the change"` |
| Review compact history | `git log --oneline --graph --decorate --all` |
| Create and switch to a branch | `git switch -c feature/name` |
| Switch branches | `git switch <branch>` |
| Integrate a branch | `git merge <branch>` |
| Download remote updates without integrating | `git fetch origin` |
| Send a branch to a remote | `git push -u origin <branch>` |

These commands are an overview, not a substitute for the chapters. Read the relevant chapter before using a command that rewrites history or discards changes.

## Prerequisites

- Git installed on your computer.
- A terminal and basic familiarity with navigating folders.
- A text editor.
- A Git-hosting account only for the remote-collaboration chapters; a GitHub account is not required to learn local Git.

## Contributing

Corrections and improvements are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening an issue or pull request. Every command and explanation should be accurate, reproducible, and clear about any destructive side effects.

## References

This guide is independently written. For authoritative command details and deeper reading, consult:

- [Official Git documentation](https://git-scm.com/docs)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [GitHub Docs: About README files](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)
- [GitHub Docs: Licensing a repository](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository)

## License

Unless otherwise noted, the written guide and educational materials in this repository are licensed under [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/). See [LICENSE](LICENSE) for the license notice. Git itself is a separate project and is not included or relicensed by this guide.
