# Chapter 09: DevOps & Linux Interview Questions and Viva prep

A compilation of the most frequently asked DevOps culture and Linux administration questions.

---

## Section 1: DevOps Culture & Tooling

### Q1: What is DevOps, and what problem does it solve?
**Answer**: DevOps is a cultural and technical philosophy that unites Software Development (Dev) and IT Operations (Ops) teams. It solves the "silo problem" where developers release software quickly without validating stability, and operators freeze changes to prevent outages. DevOps uses automation (CI/CD) to ensure code changes are released safely, frequently, and reliably.

### Q2: What are the key metrics for measuring DevOps performance?
**Answer**: The DORA (DevOps Research and Assessment) metrics are:
1. **Deployment Frequency**: How often code is successfully deployed to production.
2. **Lead Time for Changes**: Time taken for a commit to go from development to production.
3. **Change Failure Rate**: Percentage of deployments causing a failure in production.
4. **Mean Time to Restore (MTTR)**: Time taken to recover from a production outage.

### Q3: Explain Continuous Integration (CI) and Continuous Deployment (CD).
**Answer**:
* **CI (Continuous Integration)**: The practice of automatically building and testing code changes every time a developer commits code to a shared repository (Git).
* **CD (Continuous Deployment)**: The practice of automatically deploying approved, tested build packages directly to production servers without human intervention.

---

## Section 2: Linux Administration

### Q4: Explain the difference between symbolic and octal permissions.
**Answer**:
* **Symbolic permissions** use letter representations to modify access: `chmod u+x file.sh` (adds execute to user/owner).
* **Octal permissions** use numerical weights representing binary values: `chmod 755 file.sh` (sets `rwx` for user (7), `r-x` for group (5), and `r-x` for others (5)).

### Q5: What is the difference between `ps aux` and `top`?
**Answer**:
* `ps aux` displays a static, point-in-time snapshot of all active system processes.
* `top` opens an interactive, real-time interface that continuously updates CPU, memory, and task states.

### Q6: What is a soft link (symbolic link) vs. a hard link?
**Answer**:
* **Soft Link (Symlink)**: A pointer or shortcut to another file path. If the original file is deleted, the symlink breaks.
* **Hard Link**: A duplicate pointer to the same physical data block (inode) on disk. If the original file is deleted, the data remains accessible through the hard link.

### Q7: How do you verify which processes are listening on a specific port?
**Answer**: Using `ss` or `netstat` commands:
```bash
sudo ss -tulpn | grep 80
```
This filters for TCP (`-t`) and UDP (`-u`) active listening (`-l`) sockets with process ownership details (`-p`) on port 80.

---

## Section 3: Rapid-Fire Viva Prep

* **Which directory contains configuration files?** `/etc`
* **What command changes file ownership?** `chown`
* **What port does SSH use by default?** Port 22
* **What does `SIGKILL` (signal 9) do?** Forces immediate process termination.
* **Which cron scheduler flag edits cron tasks?** `crontab -e`
* **What is PID 1?** The first process started by the kernel, managed by Systemd.
* **What does `grep` do?** Searches files for pattern matches.
* **What operator appends content to a file?** `>>`
