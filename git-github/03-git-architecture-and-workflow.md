# Chapter 07 - 08: Git Architecture and Workflow

This manual details Git's internal architecture, explaining the three main areas and how files transition through them.

---

## Chapter 07: Git Architecture (The Three Areas)

A Git project consists of three main virtual zones: the **Working Directory**, the **Staging Area (Index)**, and the **Local Repository**.

```
+-----------------------------------------------------------------------+
|                              LOCAL COMPUTER                           |
|                                                                       |
|  +-------------------+      +-------------------+      +-----------+  |
|  | Working Directory | ===> | Staging Area      | ===> | Local Git |  |
|  | (Edit Files)      |      | (Prepare Stage)   |      | Repo (.git|  |
|  +-------------------+      +-------------------+      +-----------+  |
|           |                           |                      |        |
|           |       git add .           |                      |        |
|           +-------------------------->|                      |        |
|                                       |     git commit       |        |
|                                       +--------------------->|        |
+--------------------------------------------------------------|--------+
                                                               |
                                            git push           |
                                                               v
                                                      +-----------------+
                                                      | Remote Registry |
                                                      | (GitHub Cloud)  |
                                                      +-----------------+
```

1. **Working Directory**: The actual sandbox directory on your computer's filesystem containing the project files you are editing.
2. **Staging Area (Index)**: A hidden file index inside the `.git` directory. It tracks all changes scheduled to be included in your next commit. This allows you to construct a clean, logical snapshot of changes before saving.
3. **Local Repository**: The local `.git` directory that stores all commits, metadata, historical file snapshots, and branch references.

---

## Chapter 08: Git Workflow Lifecycle

Files in Git exist in one of four states: **Untracked, Unmodified, Modified, or Staged**.

```
                         [ Untracked File ]
                                 |
                                 | git add
                                 v
   +---> [ Staged ] <======== [ Modified ]
   |         |      git add      ^
   |         |                   | File edited
   |         | git commit        |
   |         v                   |
   +---> [ Unmodified ] ---------+
```

### Workflow Steps:
1. **Create or Modify a File**: The file is **Untracked** (if new) or **Modified** (if updated) in your Working Directory.
2. **Stage the File (`git add`)**: Changes are computed and moved to the **Staging Area**.
3. **Commit the Snapshot (`git commit`)**: Staged changes are permanently saved in your **Local Repository** history as a commit.
4. **Publish (`git push`)**: The commits are uploaded to a remote hosting service like GitHub.
