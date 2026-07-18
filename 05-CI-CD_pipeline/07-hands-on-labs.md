# Chapter 07: CI/CD Pipelines Hands-on Labs

Follow this comprehensive lab exercise step-by-step to create a full testing-to-deployment pipeline.

---

## Lab: Build, Test, and Deploy a Node.js Application

**Goal**: Configure a complete GitHub Actions workflow that:
1. Triggers automatically on pushes to the `main` branch.
2. Checks out code and executes unit tests.
3. If tests pass, connects to a remote VM server over SSH and deploys the application.

---

### Step 1: Create local project files
Initialize a node application workspace locally:
```bash
# Initialize node project
npm init -y

# Install express (app runtime) and jest (for testing)
npm install express
npm install --save-dev jest
```

### Step 2: Write test script configuration
Modify your `package.json` file to include a test command:
```json
"scripts": {
  "test": "jest"
}
```
Create a basic test file named `app.test.js`:
```javascript
test('Simple addition test', () => {
    expect(1 + 2).toBe(3);
});
```

---

### Step 3: Create GitHub Actions Workflow File
Create directory structure `.github/workflows/` and add a workflow file named `deploy.yml`:

```yaml
name: Node App CI-CD

on:
  push:
    branches: [ "main" ]

jobs:
  # Job 1: Run code checks
  test-and-build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run Tests
        run: npm test

  # Job 2: Deploy to remote server (runs only if test job passes)
  deploy-to-server:
    runs-on: ubuntu-latest
    needs: test-and-build
    steps:
      - name: Deploy using SSH
        uses: appleboy/ssh-action@v0.1.10
        with:
          host: ${{ secrets.VM_IP }}
          username: ${{ secrets.VM_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /var/www/my-node-app
            git pull origin main
            npm install --production
            pm2 restart all || pm2 start app.js --name "node-app"
```

---

### Step 4: Configure Repository Secrets on GitHub
Before pushing your code, go to your GitHub repository Settings -> Secrets -> Actions, and add three repository secrets:
1. `VM_IP`: The public IP address of your remote VM server.
2. `VM_USER`: The username (e.g. `ubuntu`).
3. `SSH_PRIVATE_KEY`: The SSH private key corresponding to the public key registered on your remote server (usually found in `~/.ssh/id_rsa` on your machine).

---

### Step 5: Test the Pipeline
Push your changes to GitHub:
```bash
git add .
git commit -m "feat: Add pipeline and tests"
git push origin main
```
* **Verify**: Click on the **Actions** tab inside your GitHub repository web interface. You will see a live visual graph of your workflow running the build tests and executing the remote deploy commands on your VM!
