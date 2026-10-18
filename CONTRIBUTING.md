# Contributing to Git Basics Guide

Thank you for helping improve this guide. Contributions should make Git easier to understand without sacrificing technical accuracy.

## Before you start

1. Search existing chapters and issues to avoid duplicating work.
2. For a substantial change, open an issue or discussion first so the scope can be agreed on.
3. Keep changes focused. A chapter correction, a new example, and a structural rewrite are easier to review as separate changes.

## Documentation standards

- Write in clear, natural English and prefer short paragraphs.
- Introduce a concept before using it in a command example.
- Use fenced code blocks with a language label where appropriate (`bash`, `powershell`, `text`).
- Use placeholders such as `<file>` or `<branch-name>` consistently and explain what they represent.
- State the expected result when it helps a learner verify the command.
- Explain prerequisites and context. A command that only works from a particular branch or repository state must say so.
- Label destructive or history-rewriting commands prominently. Prefer a safe inspection command before a risky operation.
- Do not imply that GitHub is required for local Git operations.
- Do not copy large sections from books, vendor documentation, or other guides. Link to authoritative references and write original explanations.
- Keep examples portable where possible. If a command is shell-specific, label the shell.

## Command safety requirements

Before adding or modifying examples involving `reset --hard`, `clean`, `restore`, `rebase`, force-push, branch deletion, or reflog recovery:

1. Verify the command against the official Git documentation.
2. Explain what state it changes and what it does not change.
3. Explain how to inspect the repository before running it.
4. Avoid recommending destructive operations as the default recovery method.
5. Use a disposable repository for examples that can discard work.

Never include secrets, access tokens, private URLs, or real user data in examples.

## Suggested workflow

```bash
git switch -c docs/short-description
# Edit the relevant Markdown files.
git diff --check
git diff -- README.md docs/
git status
```

Read every changed section as rendered Markdown and verify relative links. If you have Git installed, confirm that any command sequence described as a complete exercise works in a clean, temporary repository.

## Pull request checklist

- [ ] The change solves one clearly defined problem.
- [ ] Commands and explanations have been verified.
- [ ] Destructive behavior is explained and appropriately warned about.
- [ ] Markdown headings, tables, and code fences render correctly.
- [ ] Relative links point to files that exist.
- [ ] Existing useful content has not been removed without a reason.
- [ ] The change does not introduce copyrighted content copied without permission.

## Style for commit messages

Use concise, imperative messages. Examples:

- `docs: explain the staging area`
- `docs: add branch-merging exercise`
- `fix: correct restore command example`
- `chore: verify internal documentation links`
