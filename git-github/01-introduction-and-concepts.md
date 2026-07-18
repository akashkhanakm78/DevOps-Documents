# Chapter 01 - 06: Introduction, Needs, Version Control, Git, and GitHub

This manual covers the core concepts of Version Control Systems (VCS), the problems of working without Git, the difference between Git and GitHub, and their architectures.

---

## Chapter 01: Introduction & Version Control Need

### 1.1 The Assignmenet Doc Analogy
Imagine you are writing a college assignment. You save it as `Assignment.docx`.
The next day, you modify it and save as `Assignment_Final.docx`.
Your professor requests changes, so you save it as `Assignment_Final2.docx` -> `Assignment_Final_Updated.docx` -> `Assignment_Final_Last.docx`.

Now imagine a professional software development company with **200+ developers**, thousands of files, and millions of lines of code. Saving files with suffix names is impossible. Developers will overwrite each other's changes, delete working versions, and break production services.

Software teams need **Version Control** to:
* Track changes over time.
* Collaborate safely without overwriting each other's code.
* Maintain complete modification history.
* Easily recover or rollback to older versions if bugs occur.

---

## Chapter 02: What is Version Control?

### 2.1 Definition of Version Control System (VCS)
A **Version Control System (VCS)** is a software tool that tracks and records changes made to files over time so that specific versions can be recalled, reviewed, or restored later.

```
       +---------------------------------------------+
       |             VERSION CONTROL TIMELINE        |
       |                                             |
       |  [Version 1] -> [Version 2] -> [Version 3]  |
       |       |              |              |       |
       |    Init Commit    Add Login     Fix Bug     |
       +---------------------------------------------+
```

### 2.2 Advantages of VCS
1. **History Tracking**: Complete log of who made what change, when, and why.
2. **Easy Rollbacks**: If a new release breaks your application, you can restore the code to the last working commit instantly.
3. **Branching & Sandboxing**: Work on experimental features in isolation without modifying the main line of code.

---

## Chapter 03: Problems Without Git

Without a version control system:
* **Overwritten Files**: If Developer A and Developer B edit the same file on a shared network drive, whoever saves last overwrites the other developer's changes.
* **Code Loss**: A single disk failure can permanently destroy months of work.
* **No Collaboration Audit**: If a critical bug is introduced, there is no way to determine when it was added or by whom.

---

## Chapter 04: What is Git?

### 4.1 Definition
**Git** is a free and open-source **Distributed Version Control System (DVCS)** designed to handle everything from small to very large projects with speed and efficiency. It was created by **Linus Torvalds** (creator of Linux) in 2005.

### 4.2 Why Git is Called "Distributed"
In a **Centralized VCS** (like SVN), developers only check out a single snapshot of the files from a central server. If the server goes offline, no one can commit or view history.

In a **Distributed VCS** (like Git), every developer's local machine contains a **complete copy of the repository**, including all files, branches, and the full commit history. Work can be done offline, and if the main server crashes, any local repository can restore it.

---

## Chapter 05: What is GitHub?

### 5.1 Definition
**GitHub** is a cloud-based hosting service that stores Git repositories online and provides collaboration features.

### 5.2 Does Git Require GitHub?
**No**. Git is a standalone command-line tool that runs locally on your machine. You do not need an internet connection or a GitHub account to track files using Git. GitHub acts as a remote host to share your local Git work with others.

---

## Chapter 06: Git vs GitHub

| Feature | Git | GitHub |
| :--- | :--- | :--- |
| **What is it?** | A local software/engine. | A cloud platform/web service. |
| **Location** | Runs on your local machine. | Hosted on remote cloud servers. |
| **Interface** | Command Line Interface (CLI). | Graphical Web Interface (GUI). |
| **Primary Purpose**| Version control (tracks code history).| Repository hosting and team collaboration. |
| **Creator** | Linus Torvalds. | Acquired by Microsoft. |
