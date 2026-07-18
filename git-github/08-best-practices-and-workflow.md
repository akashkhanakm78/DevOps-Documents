# Chapter 26 - 27: Best Practices, Workflows, and DevOps Integration

This manual details industry-standard workflows, commit guidelines, and Git's role in DevOps automated deployment pipelines.

---

## Chapter 26: Git Best Practices

To maintain a healthy codebase and avoid collaboration issues, follow these rules:

1. **Commit Small, Logical Units**: Do not combine a user login bugfix and a new payment page into a single massive commit. Make two distinct, self-contained commits.
2. **Write Meaningful Commit Messages**: 
   * Avoid messages like `fix`, `changes`, or `update`.
   * Use the imperative mood: `feat: Add Stripe payment integration` instead of `Added Stripe payment`.
3. **Pull Before You Push**: Always pull the latest changes from the remote repository (`git pull`) before pushing to avoid out-of-sync conflicts.
4. **Never Commit Secrets**: Prevent passwords, API tokens, database connection strings, or private keys from entering your Git repository. Always use `.env` files and register them in `.gitignore`.
5. **Protect Main Branch**: Never commit directly to the `main` branch. Use feature branches and merge them via Pull Requests after review.

---

## Chapter 27: Real Industry DevOps Workflow

In professional DevOps environments, Git serves as the single source of truth for the entire infrastructure (GitOps) and code delivery process.

```
+------------------+     Push     +--------------+     Webhook     +-----------------+
| Developer Laptop | ===========> | GitHub Cloud | ===============> | CI/CD Server    |
| (Feature Branch) |              | (Pull Request)               | (Jenkins/Runner)|
+------------------+              +--------------+                  +--------+--------+
                                                                             | Run Unit Tests
                                                                             | Build Container
                                                                             v
                                                                    +-----------------+
                                                                    | K8s Production  |
                                                                    | Cluster         |
                                                                    +-----------------+
```

### GitOps Pipeline Lifecycle:
1. **Developer Workspace**: Developer checkout a new feature branch `feature/registration` locally.
2. **Commit & Push**: Developer commits code updates and pushes the branch to GitHub.
3. **Trigger PR**: A Pull Request is opened on GitHub. This triggers an automated pipeline (Jenkins, GitHub Actions, or GitLab CI) to download the code, compile it, run unit tests, and perform vulnerability checks.
4. **Review & Merge**: Team leads review code and approve the PR. The branch is merged into `main`.
5. **Continuous Deployment (CD)**: Merging into `main` automatically triggers the CD pipeline, building a fresh Docker image and deploying it live to Kubernetes.
