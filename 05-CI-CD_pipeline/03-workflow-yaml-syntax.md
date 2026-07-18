# Chapter 03: Workflow YAML Syntax Reference

This manual details the structure and directives used to write GitHub Actions workflow configuration files.

---

## 1. Directory Location
Workflow configurations must be stored in the root of your project directory inside the following hidden directory path:
```text
.github/workflows/my-workflow.yml
```
* **Naming**: The file must use the `.yml` or `.yaml` extension.

---

## 2. Comprehensive YAML Template

```yaml
name: Production Deployment Pipeline # Workflow name displayed in GitHub UI

# 1. Triggers: Events that start the workflow
on:
  push:
    branches: [ "main", "release/*" ] # Run when pushes occur on these branches
  pull_request:
    branches: [ "main" ] # Run when PRs are targetting main
  workflow_dispatch: # Enable manual run button in GitHub UI

# 2. Jobs: Group of execution steps
jobs:
  # Job 1: Running unit tests
  run-tests:
    name: Build & Test Application
    runs-on: ubuntu-latest # Operating System of VM runner
    steps:
      # Step 1: Use standard GitHub action to checkout git code to VM workspace
      - name: Checkout Source Code
        uses: actions/checkout@v3

      # Step 2: Use community action to setup node
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      # Step 3: Run shell commands
      - name: Install Dependencies
        run: npm ci

      - name: Execute Tests
        run: npm test

  # Job 2: Deploying to staging
  deploy-staging:
    name: Deploy to Staging VM
    runs-on: ubuntu-latest
    needs: run-tests # SEQUENTIAL: This job only runs if 'run-tests' passes
    steps:
      - name: Run Deploy Command
        run: echo "Deploying app..."
```

---

## 3. Directives Explained

* **`uses`**: Instructs the step to run a pre-packaged action from the GitHub marketplace. Syntax: `owner/repository@version` (e.g., `actions/checkout@v3`).
* **`run`**: Runs a shell command on the runner machine.
* **`needs`**: By default, jobs run in parallel. `needs` establishes a sequential dependency chain. If a prerequisite job fails, the dependent job is automatically skipped.
* **`runs-on`**: Specifies the runner OS. Options include `ubuntu-latest`, `windows-latest`, `macos-latest`, or custom tags for self-hosted runners (`self-hosted`).
