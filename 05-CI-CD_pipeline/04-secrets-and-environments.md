# Chapter 04: Secrets and Environment Variables

This manual explains how to configure environment variables and securely store passwords, tokens, and SSH keys in GitHub Secrets.

---

## 1. Environment Variables (`env`)

Environment variables allow you to configure settings without hardcoding values in your commands. You can define variables at different levels inside your YAML workflow:

```yaml
name: Variable Scopes Example

# 1. Global Level: Available to all jobs in the workflow
env:
  GLOBAL_VAR: "global_value"

jobs:
  build:
    runs-on: ubuntu-latest
    # 2. Job Level: Available to all steps inside this job
    env:
      JOB_VAR: "job_value"
    steps:
      - name: Print Variables
        # 3. Step Level: Available only to this step
        env:
          STEP_VAR: "step_value"
        run: |
          echo "Global: $GLOBAL_VAR"
          echo "Job: $JOB_VAR"
          echo "Step: $STEP_VAR"
```

---

## 2. GitHub Secrets (Secure Variables)

Never commit API keys, database passwords, Docker Hub credentials, or SSH private keys directly into your Git repository. 
* **GitHub Secrets** are encrypted variables that you configure in your GitHub repository settings.
* Once created, they are encrypted and cannot be viewed in the GitHub web interface. They are only decrypted at runtime when injected into your workflow.
* GitHub automatically masks secrets in workflow logs (replacing them with `***`) to prevent accidental leaks.

### 2.1 Accessing Secrets in YAML
Secrets are referenced using the `secrets` context object:

```yaml
steps:
  - name: Login to Docker Hub
    run: |
      # Reference secrets in shell script commands
      docker login -u "${{ secrets.DOCKER_USERNAME }}" -p "${{ secrets.DOCKER_PASSWORD }}"
```

### 2.2 How to configure Secrets on GitHub
1. Go to your repository page on GitHub.
2. Click on **Settings** -> **Secrets and variables** -> **Actions**.
3. Click **New repository secret**.
4. Enter the **Name** (e.g., `DOCKER_PASSWORD`) and paste the **Value**. Click **Add secret**.
