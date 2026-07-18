# Chapter 05: Continuous Integration (CI) Workflows

This manual provides production-ready Continuous Integration templates for building, linting, and testing Node.js, Python, and Java applications, including caching strategies to speed up pipeline execution.

---

## 1. Node.js CI Pipeline with Dependency Caching

Caching dependencies (like `node_modules`) between workflow runs prevents downloading packages on every build, reducing execution time by up to 50%.

```yaml
name: Node.js CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: 'npm' # Automatically configures npm package caching

      - name: Install dependencies
        run: npm ci

      - name: Run Linter (Code Quality Check)
        run: npm run lint

      - name: Execute Unit Tests
        run: npm run test
```

---

## 2. Python CI Pipeline

```yaml
name: Python App CI

on:
  push:
    branches: [ "main" ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
          cache: 'pip' # Auto-cache python package dependencies

      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install flake8 pytest
          if [ -f requirements.txt ]; then pip install -r requirements.txt; fi

      - name: Lint code with Flake8
        run: |
          # stop the build if there are Python syntax errors
          flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics

      - name: Run Tests with Pytest
        run: pytest
```

---

## 3. Java Spring Boot CI Pipeline

```yaml
name: Java CI with Maven

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: 'maven' # Auto-cache maven artifacts (.m2 directory)

      - name: Compile and Test with Maven
        run: mvn clean test

      - name: Package Application Archive
        run: mvn package -DskipTests
```
* **Explanation**: `mvn package -DskipTests` generates the final executable application file (e.g. `.jar` or `.war`) under the `target/` directory without running unit tests twice (since they already ran in the preceding step).
