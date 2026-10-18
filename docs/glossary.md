# Git Glossary

A compact reference for terms used throughout this guide.

| Term | Meaning |
|---|---|
| **Branch** | A movable reference to a commit, commonly used for an independent line of development. |
| **Cherry-pick** | Apply the changes introduced by selected commit(s) to the current branch as new commit(s). |
| **Clone** | Create a local repository copy from another repository, including its available history. |
| **Commit** | A recorded snapshot with metadata and parent commit references. |
| **Conflict** | A situation in which Git cannot automatically reconcile changes during an operation such as merge or rebase. |
| **Detached HEAD** | A state where `HEAD` points directly to a commit rather than naming a local branch. New commits made here may become difficult to find if no branch or tag points to them. |
| **Fetch** | Download objects and update remote-tracking references without automatically integrating them into the current branch. |
| **HEAD** | The reference that identifies the currently checked-out commit or, normally, the current branch. |
| **Index / Staging area** | The snapshot prepared for the next commit. |
| **Merge** | Integrate the histories of branches. Depending on the graph and options, the result may be a fast-forward or a merge commit. |
| **Origin** | A conventional name for the remote from which a repository was cloned. It is not required to be named `origin`. |
| **Rebase** | Replay commits onto a new base, generally creating new commit IDs for replayed commits. |
| **Reflog** | A local record of recent updates to references such as `HEAD`; useful for investigation, but not a permanent backup. |
| **Remote** | A named reference to another repository location. |
| **Remote-tracking branch** | A local reference, such as `origin/main`, recording the last fetched state of a branch on a remote. |
| **Repository** | The Git database and metadata that store commit history and related references. In everyday usage, the project directory and its `.git` data are often both called the repository. |
| **Reset** | Move the current branch/`HEAD` and optionally update the index and working tree according to the chosen mode. Some uses can discard work. |
| **Restore** | Restore working-tree files or index entries from a source such as the index or a commit. |
| **Revert** | Create a new commit that reverses the effect of an earlier commit. |
| **Stash** | A mechanism for temporarily saving selected local modifications outside normal branch history. |
| **Tag** | A reference used to mark a particular commit, often for a release. |
| **Three-way merge** | A merge that uses the common ancestor and both branch tips to combine changes. |
| **Tracking / upstream branch** | The remote branch associated with a local branch for convenient push/pull defaults. |
| **Working tree** | The checked-out files and directories where you edit the project. |

## Related chapters

- [Basic Workflow](02-basic-workflow.md)
- [Branching](04-branching.md)
- [Rebasing](06-rebasing.md)
- [Undoing Changes](07-undoing-changes.md)
- [Remote Repositories](09-remote-repositories.md)
