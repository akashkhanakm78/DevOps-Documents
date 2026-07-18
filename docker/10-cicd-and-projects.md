# Chapter 18 - 19: CI/CD Pipelines and Hands-On Projects

This manual covers integrating Docker into CI/CD pipelines (Jenkins, GitHub Actions) and walk-throughs of two real-world projects.

---

## Chapter 18: CI/CD with Docker

Automating the build, test, and deployment of Docker images is a fundamental requirement of DevOps pipelines.

### 18.1 GitHub Actions Workflow Example
Create a file named `.github/workflows/docker-ci.yml` in your repository:

```yaml
name: Docker CI/CD Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Source Code
        uses: actions/checkout@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and Push Docker Image
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/my-app:latest
```

---

## Chapter 19: Real-World Hands-On Projects

### Project 1: Multi-Tier Node.js & MongoDB App
Deploying a Node.js API with a MongoDB database backend, sharing an isolated bridge network and persistent volume.

#### Step 1: Create network and volumes
```bash
# Create custom bridge network
docker network create app-net

# Create database persistent volume
docker volume create mongo-data
```

#### Step 2: Start MongoDB
```bash
docker run -d \
  --name mongodb \
  --network app-net \
  -v mongo-data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  mongo:6.0
```

#### Step 3: Run Node.js API container
```bash
docker run -d \
  --name node-api \
  --network app-net \
  -p 3000:3000 \
  -e MONGO_URI=mongodb://admin:password@mongodb:27017/db?authSource=admin \
  node:18-alpine node -e "console.log('API Connected')"
```

---

### Project 2: Load Balanced Web App with Nginx

Distributing web traffic across multiple replication instances of a web service using Nginx as a reverse proxy.

#### Step 1: Create the Nginx Configuration (`nginx.conf`)
```nginx
events { worker_connections 1024; }

http {
    upstream backend_servers {
        server web-app-1:80;
        server web-app-2:80;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://backend_servers;
        }
    }
}
```

#### Step 2: Create `docker-compose.yml` to spin up the stack
```yaml
version: '3.8'

services:
  nginx-lb:
    image: nginx:alpine
    container_name: nginx-lb
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - web-app-1
      - web-app-2

  web-app-1:
    image: nginxdemos/hello:plain
    container_name: web-app-1

  web-app-2:
    image: nginxdemos/hello:plain
    container_name: web-app-2
```

#### Step 3: Start the Load-Balanced Stack
```bash
docker compose up -d
```
* **Verification**: Visit `http://localhost` on your host web browser. Refreshing the page will dynamically rotate traffic between the load balanced web hosts.
