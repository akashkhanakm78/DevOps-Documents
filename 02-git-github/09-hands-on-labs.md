# Chapter 28: Git & GitHub Hands-on Labs

Follow these lab exercises step-by-step to master core Git operations.

---

## Lab 1: Basic Workflow (Initialize, Stage, Commit)

**Goal**: Initialize a new repository, stage a file, and record a commit snapshot.

### Steps
```bash
# 1. Create a workspace folder
mkdir git-lab
cd git-lab

# 2. Initialize the Git repository
git init

# 3. Create a README file
echo "# Git Lab Project" > README.md

# 4. Check the repository status (should show README.md as untracked in red)
git status

# 5. Stage the file
git add README.md

# 6. Verify Staging (should show file in green ready for commit)
git status

# 7. Record the commit snapshot
git commit -m "initial: Add project README file"

# 8. Check log history
git log --oneline
```

### Expected Output for Step 8
```text
[hash] initial: Add project README file
```

---

## Lab 2: Feature Branching and Merging

**Goal**: Create an isolated branch, develop a feature, and merge it back into the main branch.

### Steps
```bash
# 1. Create and switch to a feature branch
git switch -c feature-login

# 2. Create the feature file
echo "<h1>Login Page</h1>" > login.html

# 3. Stage and commit changes
git add login.html
git commit -m "feat: Create login webpage"

# 4. Switch back to main branch
git switch main

# 5. Merge the feature-login branch
git merge feature-login

# 6. Verify that login.html exists on the main branch
ls
```

### Expected Output for Step 6
```text
README.md  login.html
```

---

## Lab 3: Intentional Merge Conflict Resolution

**Goal**: Induce a merge conflict and resolve it manually.

### Steps
```bash
# 1. Create branch-A and modify README.md
git switch -c branch-A
echo "Hello from Branch A" > README.md
git add README.md
git commit -m "update: Set header on Branch A"

# 2. Switch back to main, create branch-B and modify README.md on the same line
git switch main
git switch -c branch-B
echo "Hello from Branch B" > README.md
git add README.md
git commit -m "update: Set header on Branch B"

# 3. Switch to main and merge branch-A (should merge clean)
git switch main
git merge branch-A

# 4. Try merging branch-B (should FAIL with conflict)
git merge branch-B
```

### Console Output showing conflict
```text
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

### Resolution Steps
```bash
# 5. Open README.md in your editor. You will see:
# <<<<<<< HEAD
# Hello from Branch A
# =======
# Hello from Branch B
# >>>>>>> branch-B

# 6. Edit the file to merge both:
# Hello from Branch A and B

# 7. Save the file and stage it
git add README.md

# 8. Complete the merge
git commit -m "merge: Resolve README merge conflict between Branch A and B"
```
