# 11. Tags and Releases

## Learning objectives

Understand what tags are, create annotated tags, inspect and push tags, and distinguish Git tags from hosting-platform releases.

## What is a tag?

A tag is a reference used to mark a particular point in history, often a release. Unlike a branch, which normally moves forward as new commits are created, a version tag is generally treated as a stable label.

Git supports lightweight and annotated tags. Annotated tags store tagger metadata, a date, and a message; they are often preferred for release markers.

## List and inspect tags

```bash
git tag
git tag --list "v1.*"
git show v1.0.0
```

Replace `v1.0.0` with a tag that exists in your repository. `git show` displays the tag and the object it points to.

## Create an annotated tag

After verifying that the intended release commit is current or explicitly selected:

```bash
git status
git log --oneline -5
git tag -a v1.0.0 -m "Release v1.0.0"
```

To tag a specific commit, append its real commit ID:

```bash
git tag -a v1.0.0 <commit-id> -m "Release v1.0.0"
```

Versioning schemes such as Semantic Versioning are conventions; make sure the tag name matches your project's release policy.

## Push tags to a remote

A normal `git push` does not necessarily push local tags. Push one tag explicitly:

```bash
git push origin v1.0.0
```

Or push all local tags with:

```bash
git push origin --tags
```

Push all tags only when you intend to publish them all. Check `git tag` first.

## Delete a tag

Delete a local tag:

```bash
git tag -d v1.0.0
```

If the tag was already pushed, deleting it locally does not delete the remote tag. Remote deletion is a separate operation and may affect other users' references:

```bash
git push origin --delete v1.0.0
```

Coordinate before removing a published release tag; moving or deleting release tags can confuse users and build systems.

## Git tag versus GitHub Release

A Git tag is part of Git's reference system. A GitHub Release is a hosting-platform page associated with a tag and may include release notes and downloadable assets. Creating one is a GitHub action, not a core Git command.

## Summary

Use annotated tags to mark important commits, verify the target before tagging, and push tags intentionally. Treat published release tags as stable references.

## Further reading

- [Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
- [git-tag](https://git-scm.com/docs/git-tag)
