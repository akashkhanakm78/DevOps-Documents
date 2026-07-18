# Chapter 29 - 30: Interview Preparation & Rapid-Fire Q&As

A compilation of the most frequent Git & GitHub interview questions, viva questions, and high-speed exam revisions.

---

## Chapter 29: Most Important Interview Q&As

### Q1: What is the difference between `git fetch` and `git pull`?
**Answer**:
* `git fetch` downloads the latest commits, branches, and tags from the remote repository to your local repository, but **does not merge** them into your working directory files.
* `git pull` downloads the remote changes and **immediately merges** them into your current active local branch. (Pull = Fetch + Merge).

### Q2: What is the difference between `git reset` and `git revert`?
**Answer**:
* `git reset` moves the branch pointer backward to a previous commit, rewriting the commit history. It deletes commits from the branch history. (Should only be used on local, unpushed branches).
* `git revert` creates a brand new commit that contains the exact opposite changes of a specified commit, undoing it. It does not rewrite or alter the existing history. (Safe to use on shared public branches).

### Q3: What is `HEAD` in Git?
**Answer**: `HEAD` is a reference pointer that points to the latest commit on your currently checked-out branch. It indicates where your working directory stands.

### Q4: What is a "detached HEAD" state?
**Answer**: A state that occurs when you checkout a specific commit hash or a tag directly (e.g., `git checkout <commit-hash>`) instead of a local branch. In this state, `HEAD` points directly to a commit. Any new commits made while detached are not associated with any branch and can be lost during garbage collection unless a new branch is created to save them.

### Q5: Why is Git essential in DevOps and CI/CD pipelines?
**Answer**: Git acts as the single source of truth for code and infrastructure configurations. Merging code into Git branches triggers webhooks that initiate automated build, test, and deploy pipelines. This ensures auditability, reproducibility, and instant rollbacks.

---

## Chapter 30: Rapid-Fire Viva Prep & 30-Sec Revision

### 30-Sec Exam Revision Summary
> Git is a distributed version control system that tracks modifications made to project source code. Developers edit files in the **Working Directory**, prepare snapshots using `git add` to place them in the **Staging Area**, save those snapshots locally with `git commit`, and publish changes to remote hosts like **GitHub** via `git push`. Git enables lightweight branching, merging, and merge conflict resolution, forming the core collaboration engine for modern DevOps pipelines.

### Rapid-Fire Viva Q&As
* **What command initializes a repository?** `git init`
* **What command displays repository status?** `git status`
* **How do you stage all files in the current folder?** `git add .`
* **How do you commit staged changes?** `git commit -m "message"`
* **How do you create and switch to a branch simultaneously?** `git switch -c <name>`
* **How do you combine changes from another branch?** `git merge <name>`
* **How do you upload local commits to GitHub?** `git push`
* **How do you download and merge commits from GitHub?** `git pull`
* **What file is used to ignore tracking files?** `.gitignore`
* **What command temporarily saves uncommitted work?** `git stash`
* **What folder contains all Git history configurations?** The hidden `.git` folder.
* **Who created Git?** Linus Torvalds in 2005.
