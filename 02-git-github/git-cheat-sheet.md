# Git & GitHub Commands Cheat Sheet

Quick command reference sheet for common Version Control tasks.

---

## 1. Setup & Configuration
```bash
git config --global user.name "John Doe"         # Configure global commit username
git config --global user.email "john@example.com"# Configure global commit email
git config --global init.defaultBranch main      # Set default initial branch name to main
git config --list                               # List active configurations
```

## 2. Basic Local Workflow
```bash
git init                                        # Initialize a local Git repository
git status                                      # Show working directory & staging status
git add index.html                              # Stage a single file
git add .                                       # Stage all changes in current folder
git commit -m "feat: Add authentication"        # Commit staged files with log message
git log --oneline                               # View compact commit history list
git diff                                        # View unstaged changes
```

## 3. Branches & Merging
```bash
git branch                                      # List local branches
git switch -c feature-name                      # Create and switch to new branch
git switch main                                 # Switch back to main branch
git merge feature-name                          # Merge feature-name into active branch
git branch -d feature-name                      # Delete a merged branch
git branch -D feature-name                      # Force delete an unmerged branch
```

## 4. Remote Synchronization
```bash
git remote add origin <repo-url>                # Connect local repo to remote URL
git remote -v                                   # List connected remote URLs
git push -u origin main                         # Push local commits to remote main branch
git pull origin main                            # Download remote changes and merge
git fetch origin                                # Download remote changes without merging
git clone <repo-url>                            # Download repository and history to local
```

## 5. Advanced Utility
```bash
# Stashing
git stash                                       # Temporarily save uncommitted changes
git stash list                                  # List saved stashes
git stash pop                                   # Restore most recent stash and remove it

# Tagging
git tag v1.0.0                                  # Create lightweight release tag
git tag -a v1.0.0 -m "Release v1.0.0"            # Create annotated release tag
git push origin --tags                          # Push tags to GitHub

# Undoing
git restore <file>                              # Discard local file modifications
git restore --staged <file>                     # Unstage a file, keeping local edits
git reset --hard HEAD                           # Discard ALL uncommitted changes completely
```
