# Chapter 06: Continuous Deployment (CD) Workflows

This manual covers advanced deployment workflows: building and publishing Docker container images to registries and executing automated remote deployments to VM host servers over SSH.

---

## 1. Building and Pushing Docker Images to Docker Hub

This pipeline builds a Docker image from a local Dockerfile, tags it with the Git commit SHA (to ensure traceability), and pushes it to Docker Hub.

```yaml
name: Publish Docker Image

on:
  push:
    branches: [ "main" ]

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      # Set up Docker Buildx (advanced docker build features)
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      # Log in to Docker Hub using encrypted repo secrets
      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Build and push the image
      - name: Build and Push Image
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          # Tags the image as 'latest' and as the specific Git Commit SHA
          tags: |
            ${{ secrets.DOCKER_USERNAME }}/my-app:latest
            ${{ secrets.DOCKER_USERNAME }}/my-app:${{ github.sha }}
```

---

## 2. Deploying directly to a Virtual Machine over SSH

In classic deployments, the pipeline must log into a remote VM server via SSH and execute a bash script (e.g., pulling a new Docker image or pulling files from Git and reloading Nginx/PM2).

```yaml
name: Deploy to VM

on:
  push:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      # Use SSH Action to execute commands remotely
      - name: Execute Remote Deploy Commands
        uses: appleboy/ssh-action@v0.1.10
        with:
          host: ${{ secrets.VM_IP }}               # Remote VM Server IP
          username: ${{ secrets.VM_USER }}         # SSH Username (e.g. ubuntu, root)
          key: ${{ secrets.SSH_PRIVATE_KEY }}      # Server's SSH Private Key
          port: 22                                 # SSH Port
          script: |
            # 1. Navigate to application folder
            cd /var/www/my-app
            
            # 2. Pull latest code from GitHub
            git pull origin main
            
            # 3. Reinstall production node packages
            npm install --production
            
            # 4. Restart application in PM2
            pm2 restart my-api
```
* **Explanation**: The `appleboy/ssh-action` establishes a secure encrypted SSH tunnel using your configured SSH private key, authenticates to the VM, executes the script block, prints the output to your GitHub Actions console log, and closes the connection.
