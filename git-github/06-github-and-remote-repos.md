# Chapter 17 - 21: GitHub, Remote Repositories, and Team Collaboration

This manual details how to configure remote repositories, synchronize code, and use collaborative features like Pull Requests and Forks.

---

## Chapter 17: GitHub Collaborative Features

GitHub provides a suite of collaborative features on top of Git:
* **Issues**: Project management tracking boards for bug reports, task lists, and feature requests.
* **Pull Requests (PRs)**: Discussion forums where developers review proposed code changes before merging.
* **GitHub Actions**: Built-in Continuous Integration/Continuous Deployment (CI/CD) pipelines.

---

## Chapter 18: Remote Repositories

A **remote repository** is a version of your project hosted on the internet (e.g., GitHub, GitLab).

### 18.1 Managing Remotes
```bash
# Add a remote repository pointer (named 'origin')
git remote add origin https://github.com/username/my-repo.git

# View configured remote URLs
git remote -v
```

### 18.2 Synchronizing Changes
```bash
# 1. Push: Upload local commits to remote GitHub
git push -u origin main
# (The '-u' flag sets the default upstream branch for future pushes/pulls)

# 2. Pull: Download latest changes from remote and merge immediately
git pull origin main

# 3. Fetch: Download changes from remote but DO NOT merge them
git fetch origin
```
* **Key Difference**: `git pull` is a shortcut for running `git fetch` followed immediately by `git merge`. `git fetch` is safer because it lets you inspect the downloaded commits (`git log origin/main`) before merging them manually.

---

## Chapter 19 - 21: Collaborative Workflows

### 19.1 Forking Workflow (Open Source Contributions)
A **Fork** is a copy of a repository hosted on your personal GitHub account. The workflow is as follows:

```
  [ Original Repository ] (Read-only)
             |
             | Fork (via GitHub UI)
             v
  [ Your Personal Fork ] (Read-write)
             |
             | git clone
             v
  [ Local Machine ] (Work, commit, push)
             |
             | git push
             v
  [ Your Personal Fork ]
             |
             | Pull Request (PR)
             v
  [ Original Repository ] (Review & Merge)
```

### 19.2 Pull Requests (PR)
A **Pull Request (PR)** notifies team members that you have completed a feature and are requesting to merge it into the main repository. 
1. Recommends running unit tests automatically on the branch (CI/CD checks).
2. Allows developers to comment on specific lines of code.
3. Once approved, the branch is merged into `main` using GitHub's web interface.
