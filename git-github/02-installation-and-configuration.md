# Chapter 09 - 10: Installing and Configuring Git

This manual covers installing Git on Windows, Linux, and macOS, and setting up global environment variables.

---

## Chapter 09: Installing Git

### 1. Windows Installation
1. Download the Git for Windows installer from [git-scm.com](https://git-scm.com/).
2. Run the `.exe` installer.
3. Keep default settings (recommended), ensuring "Git Bash" is selected. This provides a Linux-like terminal environment on Windows.

### 2. Linux Installation (Debian/Ubuntu)
```bash
sudo apt update
sudo apt install -y git
```

### 3. macOS Installation
Using Homebrew:
```bash
brew install git
```

### 4. Verification Check
Open your terminal (or Git Bash on Windows) and run:
```bash
git --version
# Expected Output: git version 2.x.x
```

---

## Chapter 10: Git Configuration

Before making commits, you must tell Git who you are. This metadata is permanently attached to every commit you make.

### 10.1 Setting Global Author Details
Run the following commands, substituting your actual name and email:

```bash
# Set your global username
git config --global user.name "John Doe"

# Set your global email
git config --global user.email "johndoe@example.com"
```

### 10.2 Setting Default Init Branch (Best Practice)
Historically, Git initialized the default branch as `master`. In modern DevOps environments, the default branch name is configured to `main`:
```bash
git config --global init.defaultBranch main
```

### 10.3 Verifying Configurations
To list all active configurations:
```bash
git config --list
```
To verify a specific parameter:
```bash
git config user.name
```
