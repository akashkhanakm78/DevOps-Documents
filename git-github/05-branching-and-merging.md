# Chapter 14 - 16: Branching, Merging, and Conflict Resolution

This manual explains how Git branches work, how to merge them, and how to resolve merge conflicts.

---

## Chapter 14: Branching

A **branch** in Git is a lightweight pointer to a specific commit. Branching allows developers to work on new features or bug fixes in isolation from the stable main codebase.

```
       [ C1 ] ---> [ C2 ] ---> [ C3 ]  <-- main branch
                     |
                     +---> [ C4 ] ---> [ C5 ]  <-- feature branch
```

### 14.1 Branch Management Commands

```bash
# 1. Create a new branch
git branch feature-login

# 2. Switch to a branch
git switch feature-login
# (or older syntax: git checkout feature-login)

# 3. Create AND switch to a new branch in one command (Recommended)
git switch -c feature-payment

# 4. List all local branches (Active branch marked with *)
git branch

# 5. Delete a branch (only if it has been merged)
git branch -d feature-login

# 6. Force delete a branch (even if unmerged)
git branch -D feature-login
```

---

## Chapter 15: Merging Branches

Once you complete development on a feature branch, you merge those changes back into the main branch.

```bash
# Step 1: Switch to the target branch (usually main)
git switch main

# Step 2: Merge the feature branch
git merge feature-payment
```

### 15.1 Merge Types
* **Fast-Forward Merge**: Occurs if no commits have been made on the `main` branch since the feature branch was created. Git simply moves the `main` branch pointer forward to the latest commit on the feature branch.
* **Three-Way Merge (Merge Commit)**: Occurs if both branches have diverged (new commits exist on both `main` and the feature branch). Git generates a new "Merge Commit" that ties the history of both branches together.

---

## Chapter 16: Resolving Merge Conflicts

### 16.1 When Do Conflicts Occur?
A merge conflict occurs when developers modify the **same line** of the **same file** on two different branches, and then try to merge them. Git does not know which version to keep and halts the merge.

### 16.2 Conflict Markers
Git marks the conflicted areas in the affected files using conflict markers:

```html
<<<<<<< HEAD
<h1>Welcome back, Admin</h1>
=======
<h1>Welcome back, User</h1>
>>>>>>> feature-login
```
* `<<<<<<< HEAD`: Indicates the code on your current active branch (the branch you are merging *into*).
* `=======`: Separator between the conflicting versions.
* `>>>>>>> feature-login`: Indicates the incoming code from the feature branch.

### 16.3 Step-by-Step Resolution
1. Open the conflicted file in your text editor.
2. Manually edit the file to choose the correct code (or combine them).
3. **Remove** the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
4. Stage the resolved file: `git add <filename>`
5. Complete the merge commit: `git commit -m "merge: Resolve login header conflict"`
