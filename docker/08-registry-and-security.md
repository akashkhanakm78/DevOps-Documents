# Chapter 13 & 17: Registry and Security

This manual details how to host, tag, push, and pull images across registries and outlines best security practices for production containers.

---

## Chapter 13: Docker Registries

A **Docker Registry** is a storage and distribution system for named Docker images. 

### 13.1 Docker Hub
The default public registry hosted by Docker.
```bash
# Step 1: Login to Docker Hub
docker login -u <username>

# Step 2: Tag your local image with your Docker Hub username
docker tag local-app:latest username/local-app:v1.0

# Step 3: Push the tagged image to Docker Hub
docker push username/local-app:v1.0
```

### 13.2 Running a Private Local Registry
You can run a private, self-hosted registry container on your local network:
```bash
# Spin up a local registry container on port 5000
docker run -d -p 5000:5000 --restart=always --name registry registry:2

# Tag an image to target the local registry
docker tag my-app:latest localhost:5000/my-app:v1.0

# Push the image to your local registry
docker push localhost:5000/my-app:v1.0
```

---

## Chapter 17: Docker Security

Security is critical when deploying containers in production. Because containers share the host kernel, a compromised container could lead to host access if misconfigured.

### 17.1 Security Best Practices

1. **Run as Non-Root User**:
   * By default, Docker runs container processes as `root` (UID 0). If an attacker escapes the container, they inherit root privileges on the host.
   * Add a non-root user in your Dockerfile and switch to it using the `USER` directive.
2. **Limit Resources**:
   * Restrict CPU and memory usage to prevent Denial of Service (DoS) attacks from noisy neighbors.
   * Command flags: `docker run --memory="512m" --cpus="1.5" nginx`
3. **Read-Only Root Filesystem**:
   * Prevent attackers from modifying files or downloading malicious scripts inside the container runtime filesystem.
   * Command flags: `docker run --read-only --tmpfs /tmp nginx`
4. **Disable Privileged Containers**:
   * Never run containers with the `--privileged` flag in production. This grants the container direct access to all device drivers on the host.
5. **Scan Images for Vulnerabilities**:
   * Scan your images for outdated packages and vulnerabilities before deploying.
   * Command: `docker scout quickview <image>` or use scanners like Trivy.
