# Chapter 08: CI/CD & GitHub Actions Interview Prep

A compilation of the most frequently asked CI/CD pipeline and GitHub Actions interview questions.

---

## Section 1: CI/CD Pipeline Concepts

### Q1: What is the difference between Continuous Delivery and Continuous Deployment?
**Answer**:
* **Continuous Delivery**: Automated processes compile, test, and package code into deployable artifacts (like container images or zip archives). The final step of deploying to production requires a **manual approval** or click.
* **Continuous Deployment**: Fully automated deployment to production. Every code change that passes all validation and testing stages is **automatically deployed** directly to live systems without manual human intervention.

### Q2: What is the benefit of caching dependencies in CI/CD?
**Answer**: Caching downloads package dependencies (like `node_modules` or Maven `.m2` directories) to a cache storage bucket. In subsequent runs, the pipeline retrieves the packages from the cache instead of downloading them from the internet, which reduces build times, network bandwidth usage, and runner usage costs.

---

## Section 2: GitHub Actions Core Syntax

### Q3: What is the difference between `uses` and `run` in a workflow step?
**Answer**:
* **`uses`**: Runs a pre-built Action, which is a reusable chunk of code (often hosted on a public GitHub repository or the marketplace). Example: `uses: actions/checkout@v3`.
* **`run`**: Runs a custom shell command or script directly on the virtual machine runner. Example: `run: npm run test`.

### Q4: By default, do jobs in a workflow run sequentially or in parallel? How do you change this?
**Answer**: By default, multiple jobs run **in parallel** (simultaneously) to save execution time. To run them sequentially, use the `needs` directive to specify dependencies. A job with `needs: run-tests` will wait until `run-tests` finishes successfully before starting.

### Q5: What is the difference between a GitHub-hosted runner and a self-hosted runner?
**Answer**:
* **GitHub-hosted runner**: A clean, short-lived virtual machine provisioned and managed by GitHub (running on Azure). It is auto-updated but cannot access private local network environments.
* **Self-hosted runner**: A machine or container you provision, manage, and scale on your own infrastructure (AWS, local VM, or physical hardware). It can access local networks but requires manual maintenance.

---

## Section 3: Scenario-based Troubleshooting

### Q6: How do you prevent sensitive API keys and SSH credentials from leaking in build logs?
**Answer**:
1. Configure them as **GitHub Secrets** in repository settings rather than writing them directly in YAML files.
2. Reference them using the secrets context: `${{ secrets.MY_TOKEN }}`.
3. GitHub automatically detects secrets and masks them in action output logs with `***`.

### Q7: If a deployment job fails because of a network timeout during SSH login, how do you debug it?
**Answer**:
1. Verify the runner VM has outbound network access to port 22 on the target host.
2. Ensure the remote server's firewall (or cloud security group rules) allows incoming connections on port 22 from GitHub runner IP ranges (or use a self-hosted runner within the private subnet).
3. Validate that the SSH private key secret matches the public key registered in `~/.ssh/authorized_keys` on the target host.
