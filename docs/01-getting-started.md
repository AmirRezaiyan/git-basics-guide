# 1. Getting Started with Git

## Learning objectives

By the end of this chapter, you should be able to explain what Git does, check whether it is installed, configure your author identity, initialize a local repository, and clone an existing repository.

## What is Git?

Git is a distributed version control system. It records a project's history as commits, lets you compare changes, and supports parallel work through branches. A local Git repository contains its own history, so many operations—such as committing, branching, and inspecting history—work without an internet connection.

Git and GitHub are not the same thing:

- **Git** is the version control system that runs on your computer.
- **GitHub** is a hosting and collaboration platform for Git repositories. Other hosting services and self-hosted remotes are also possible.

## Install Git

Use the official download page and follow the instructions for your operating system: [git-scm.com/downloads](https://git-scm.com/downloads).

After installation, open a new terminal and verify it:

```bash
git --version
```

The exact version number depends on the installed release. If the terminal cannot find `git`, confirm installation and make sure Git is available on your `PATH`; then restart the terminal.

## First-time configuration

Set the name and email that Git should record as the author of your commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Replace the example values with your intended author information. These settings affect commit metadata; they are not the credentials used to authenticate to GitHub.

Check the values:

```bash
git config --global --get user.name
git config --global --get user.email
git config --list --show-origin
```

The `--global` option stores settings for your user account. A repository-specific setting can be created by running `git config user.name "Different Name"` inside that repository; local repository configuration takes precedence over global configuration. Review `git config --list --show-origin` before sharing terminal output, because configuration can contain private paths or other local details.

### Optional: use `main` for new repositories

```bash
git config --global init.defaultBranch main
```

This configures the default initial branch name for repositories you create with `git init` in the future. It does not rename branches in existing repositories.

## Create a new local repository

Use `git init` when you already have a project folder that is not yet tracked by Git. The following file-creation commands use Bash/POSIX shell syntax. In Windows PowerShell, you can create or replace a UTF-8 text file with `Set-Content -Encoding utf8 notes.txt "Hello, Git"`; append a line with `Add-Content -Encoding utf8 notes.txt "Another line"`. Windows PowerShell 5.1 and PowerShell 7 have different default text encodings, so using an editor or explicitly specifying UTF-8 avoids confusing text-file diffs.

```bash
mkdir hello-git
cd hello-git
git init
git status
```

Git creates a repository in the current folder. Depending on configuration and Git version, the initial branch may have a different name if you have not configured `init.defaultBranch`.

Create a small file, then record it:

```bash
echo "Hello, Git" > README.md
git add README.md
git commit -m "Add initial README"
```

If a commit fails because your identity is not configured, complete the configuration steps above and retry. If Git says there is nothing to commit, check `git status` to confirm whether there are any changes.

## Clone an existing repository

Use `git clone` when a repository already exists and you want a local copy that includes its history.

```bash
git clone https://github.com/OWNER/REPOSITORY.git
cd REPOSITORY
git status
git remote -v
```

Replace `OWNER/REPOSITORY` with the real repository path. A clone normally creates a remote named `origin` automatically. Public repositories can usually be cloned without signing in; private repositories require suitable access and authentication.

## Basic navigation and help

```bash
pwd                 # macOS/Linux shells; PowerShell also supports Get-Location
ls                  # macOS/Linux shells; PowerShell also supports Get-ChildItem
git help status
git status --help
```

The `pwd` and `ls` examples are shell commands, not Git commands. In Windows PowerShell, use `Get-Location` and `Get-ChildItem` if preferred. Git's built-in help can open a manual page or browser depending on your environment.

## A safe first-session checklist

1. Run `git --version`.
2. Configure `user.name` and `user.email`.
3. Create a disposable directory and run `git init`.
4. Add one file and make one commit.
5. Run `git status` and `git log --oneline`.
6. Delete the practice directory only after confirming it contains no work you need.

## Common mistakes

- **Confusing Git with GitHub:** local commits do not require a hosting account.
- **Using the wrong folder:** run `git status` to see which repository you are in.
- **Assuming `git init` uploads files:** it creates a local repository; it does not publish anything.
- **Putting a password in a remote URL:** avoid embedding passwords or tokens in URLs, scripts, or shell history. Use your host's supported authentication method.

## Summary

Git records local history; GitHub hosts repositories and enables collaboration. `git init` starts tracking a folder, `git clone` copies an existing repository, and `git config` configures your commit identity.

## Further reading

- [Git installation](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [First-time Git setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
- [git-init](https://git-scm.com/docs/git-init)
- [git-clone](https://git-scm.com/docs/git-clone)
