# GitHub Actions Cheat Sheet

Quick YAML reference block and syntax card for GitHub Actions CI/CD workflows.

---

## 1. Directory Location
```text
.github/workflows/deploy.yml
```

## 2. Trigger Events (`on:`)
```yaml
on:
  push:
    branches: [ "main" ]                     # Trigger on pushes to main
  pull_request:
    branches: [ "main" ]                     # Trigger on PR targetting main
  workflow_dispatch:                         # Enable manual run trigger button
```

## 3. Dependency Job Ordering (`needs:`)
By default, jobs run in parallel. Use `needs` for sequential execution:
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: npm test

  deploy:
    runs-on: ubuntu-latest
    needs: test                              # Run ONLY if the test job passes
```

## 4. Setup Actions Reference
```yaml
# 1. Checkout repository code to VM runner workspace
- name: Checkout Code
  uses: actions/checkout@v3

# 2. Setup Node.js environment
- name: Setup Node
  uses: actions/setup-node@v3
  with:
    node-version: 18

# 3. Setup Java JDK environment
- name: Setup Java JDK
  uses: actions/setup-java@v3
  with:
    java-version: '17'
    distribution: 'temurin'
```

## 5. Security & Credentials References
Inject encrypted variables from GitHub Secrets:
```yaml
env:
  API_URL: "https://api.myapp.com"           # Standard environment variable

steps:
  - name: Use Secrets
    env:
      DB_PASSWORD: ${{ secrets.DB_PASSWORD }} # Decrypt secret variable
    run: ./run_migration.sh
```

## 6. Matrix Builds (Multi-version testing)
Run a job across multiple version configurations concurrently:
```yaml
strategy:
  matrix:
    node-version: [14.x, 16.x, 18.x]
    os: [ubuntu-latest, windows-latest]
```
Reference version variables in the build steps:
```yaml
runs-on: ${{ matrix.os }}
steps:
  - uses: actions/setup-node@v3
    with:
      node-version: ${{ matrix.node-version }}
```
