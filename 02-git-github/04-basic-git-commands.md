# Chapter 11 - 13: Basic Git Commands and Commit Lifecycle

This manual details how to initialize a repository, monitor changes, stage and commit files, view logs, and discard modifications.

---

## Chapter 11: Creating a Repository (`git init`)

To start tracking a project, navigate to your root folder and run:
```bash
git init
```

### The Hidden `.git` Directory
Running `git init` creates a hidden folder named `.git` in your project root. **Never delete this folder**.
* It stores all your project's history, branches, configuration settings, and object databases.
* Removing `.git` instantly converts your project back into a normal, untracked folder, erasing all history and commits.

---

## Chapter 12: Core Basic Commands

### 12.1 `git status`
Shows the current state of the working directory and the staging area.
* Displays untracked files (in red).
* Displays modified files not yet staged.
* Displays staged files ready for commit (in green).

### 12.2 `git add`
Moves changes from the working directory to the staging area.
```bash
# Stage a single file
git add index.html

# Stage all files and directories in the current folder
git add .
```

### 12.3 `git commit`
Saves the staged snapshot as a permanent checkpoint in the repository history.
```bash
# Create a commit with a descriptive log message
git commit -m "feat: Add user login system"
```

### 12.4 `git log`
Displays the project's commit history, listing author, date, unique hash, and commit message.
```bash
# View complete logs
git log

# View simplified history (one line per commit)
git log --oneline
```

### 12.5 `git diff`
Shows line-by-line differences of file modifications that have not yet been staged.
```bash
git diff
```

---

## Chapter 13: Undoing Changes

If you make a mistake, Git provides commands to safely discard changes.

```bash
# 1. Discard modifications made to a file in the Working Directory
git restore index.html

# 2. Unstage a file (moves it back to Working Directory, keeping your changes)
git restore --staged index.html

# 3. Discard all uncommitted changes completely
git reset --hard HEAD
```
* **Caution**: `git reset --hard` will permanently delete all uncommitted changes in your Working Directory. Use with extreme care.
