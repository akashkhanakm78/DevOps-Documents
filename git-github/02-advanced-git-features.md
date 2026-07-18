# Chapter 22 - 25: Advanced Git Features

This manual explores advanced version control commands: ignoring files, release tagging, stashing uncommitted changes, and rebasing.

---

## Chapter 22: `.gitignore` Configurations

A `.gitignore` file is a plain text file containing rules specifying which files and folders Git should ignore. This prevents temporary files, dependency folders, and build artifacts from cluttering repository history.

### 22.1 Example `.gitignore` File
Create a file named `.gitignore` in your root folder:
```text
# Ignore local environment variables (security keys)
.env
.env.local

# Ignore package dependencies (extremely large, generated dynamically)
node_modules/

# Ignore compiler build outputs
dist/
build/
bin/

# Ignore all log files
*.log
```

---

## Chapter 23: Git Tags

**Tags** are bookmarks to specific points in history, typically used to mark release versions (e.g., `v1.0.0`, `v2.0.0`).

```bash
# 1. Create a lightweight tag pointing to current commit
git tag v1.0.0

# 2. Create an annotated tag (includes author, date, and message - recommended)
git tag -a v1.1.0 -m "Release v1.1.0 with payment support"

# 3. Push tags to remote GitHub (tags are not pushed by default)
git push origin --tags
```

---

## Chapter 24: Git Stash

If you are working on a new feature and need to switch branches quickly to fix an urgent bug, but you aren't ready to commit your unfinished work, you use `git stash`.

```bash
# 1. Save uncommitted changes and clean the working directory
git stash

# 2. List all saved stashes
git stash list

# 3. Restore and remove the most recently saved stash
git stash pop

# 4. Drop/delete a saved stash without applying it
git stash drop
```

---

## Chapter 25: Git Rebase

**Rebasing** is the process of moving or reapplying a sequence of commits to a new base commit. 

```
  Before Merge:
  main:     [C1] -> [C2] -> [C3]
  feature:    \---> [C4] -> [C5]

  After Merge (creates merge commit C6):
  main:     [C1] -> [C2] -> [C3] ---------> [C6]
  feature:    \---> [C4] -> [C5] -----------/

  After Rebase (rewrites history for linear timeline):
  main:     [C1] -> [C2] -> [C3] -> [C4'] -> [C5']
```

### 25.1 Merge vs Rebase Comparison

| Command | History | Safety | Best Practice |
| :--- | :--- | :--- | :--- |
| **`git merge`** | Non-destructive. Preserves complete chronological history of branches. | High (safe for shared branches). | Use for merging feature branches into main when keeping audit history matters. |
| **`git rebase`** | Destructive. Rewrites history by moving commits, producing a linear timeline. | Low (can cause issues on shared branches). | Use to update local feature branches with latest commits from `main` before submitting PRs. |

> [!WARNING]
> **The Golden Rule of Rebasing**: Never rebase branches that have been pushed to a public remote repository and are shared with other developers. Rebasing rewrites commits, which will break other developers' local tracking branches.
