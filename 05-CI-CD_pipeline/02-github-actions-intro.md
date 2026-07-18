# Chapter 02: GitHub Actions Introduction & Architecture

This manual explains how GitHub Actions works, its component hierarchy, and the difference between GitHub-hosted and self-hosted runners.

---

## 1. What is GitHub Actions?

**GitHub Actions** is a Continuous Integration and Continuous Deployment (CI/CD) platform integrated directly into GitHub. It allows you to automate your software development workflows directly in the repository where your code resides.

---

## 2. Core Architecture Components

GitHub Actions processes are defined using YAML files and structured in a hierarchy of components:

```
  [ Workflow ] (Triggered by events like Push/PR)
      |
      +---> [ Job 1 (Runs on VM Runner) ]
      |        |
      |        +---> [ Step 1: Checkout Code ]
      |        +---> [ Step 2: Install Node.js ]
      |        +---> [ Step 3: Run Tests ]
      |
      +---> [ Job 2 (Runs on separate VM Runner) ]
```

1. **Workflows**: An automated procedure added to your repository. Workflows are defined in YAML files inside the `.github/workflows/` directory.
2. **Events**: A specific activity that triggers a workflow. Examples: a push to `main`, opening a Pull Request, or a manual trigger (`workflow_dispatch`).
3. **Jobs**: A set of steps executed on the same runner. By default, multiple jobs in a workflow run in parallel.
4. **Steps**: An individual task that runs commands. Steps in a job run sequentially on the same runner, allowing them to share data.
5. **Actions**: Reusable packages of code (written in JavaScript or Docker containers) that perform common tasks (e.g., checking out code, setting up Java JDK).
6. **Runners**: The virtual machine server that executes the jobs defined in your workflow.

---

## 3. Runners Compared: GitHub-Hosted vs. Self-Hosted

When a job runs, it requires a machine to execute the commands. You can choose where it runs:

| Feature | GitHub-Hosted Runners | Self-Hosted Runners |
| :--- | :--- | :--- |
| **Hosting** | Managed by GitHub in Microsoft Azure cloud. | Hosted on your own physical servers or VMs (AWS, local). |
| **Maintenance** | Auto-managed. GitHub updates OS patches and software. | Managed by your DevOps team. You must install dependencies. |
| **Clean Environment**| High. Spins up a fresh, clean VM for every job. | Low. Files persist between jobs unless cleaned manually. |
| **Private Network** | Cannot access internal private networks directly. | Can easily access databases/deploy to local network. |
| **Cost** | Free tier available; charges per minute beyond limits. | Free software; you pay only for your infrastructure. |
