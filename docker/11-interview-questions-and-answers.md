# Chapter 20: 100+ Interview, Viva, and Exam Q&As

A compilation of the most frequently asked Docker questions, answered concisely and accurately for exams, viva-voce, and interviews.

---

## Section 1: Core Concepts & Architecture

### Q1: What is a Docker container?
**Answer**: A container is a lightweight, standalone, executable software package that includes everything needed to run an application (code, runtime, system tools, system libraries, and settings) running in isolation on a shared host OS kernel.

### Q2: What is a Docker image?
**Answer**: A Docker image is a read-only, multi-layered template with instructions for creating a Docker container. It is built from a Dockerfile.

### Q3: What is the difference between `docker run` and `docker start`?
**Answer**: `docker run` creates a *new* container instance from an image and starts it. `docker start` resumes execution of an *existing, stopped* container.

### Q4: Explain the differences between a VM and a Container.
**Answer**: A VM abstracts physical hardware and includes a complete guest Operating System running on a hypervisor. A container abstracts the host Operating System and shares the host kernel, making it lightweight, faster to start, and highly resource-efficient.

### Q5: What Linux kernel namespaces does Docker use?
**Answer**: 
* `pid` (Process ID isolation)
* `net` (Network interface isolation)
* `mnt` (Filesystem mount isolation)
* `ipc` (System IPC resources isolation)
* `uts` (Hostname and domain name isolation)
* `user` (User and group ID isolation)

### Q6: What is a Control Group (cgroup)?
**Answer**: A Linux kernel feature used by Docker to limit, account for, and isolate the physical resource usage (such as CPU, Memory, Disk I/O, Network) of a collection of processes (containers).

### Q7: What is Docker Hub?
**Answer**: A cloud-based public registry service provided by Docker for finding, sharing, and storing container images.

### Q8: What is a Dockerfile?
**Answer**: A text document containing a sequential list of instructions and commands that Docker uses to build a Docker image.

### Q9: What is Docker Compose?
**Answer**: A tool for defining and running multi-container applications using a single YAML configuration file.

### Q10: What is a Docker daemon?
**Answer**: The background service (`dockerd`) running on the host that listens for API requests and manages Docker objects like images, containers, networks, and volumes.

---

## Section 2: Storage & Networking

### Q11: What are Docker volumes?
**Answer**: A persistent storage mechanism managed by Docker (stored under `/var/lib/docker/volumes/` on Linux) that persists independently of a container's lifecycle.

### Q12: What is a bind mount?
**Answer**: A storage mechanism that mounts a user-specified directory or file from the host filesystem directly into the container filesystem.

### Q13: What is the default network driver in Docker?
**Answer**: The **Bridge** network driver. It sets up a private virtual network on the host, assigning IP addresses to each container.

### Q14: What is the Host network driver?
**Answer**: A network driver that removes isolation between the host and the container, letting the container use the host's network interfaces directly.

### Q15: Explain the Overlay network driver.
**Answer**: An overlay network connects multiple Docker daemons across different host machines, enabling containers in a Swarm or cluster to communicate.

### Q16: What is a tmpfs mount?
**Answer**: A volatile storage mechanism that mounts a directory in the host's system memory (RAM). When the container stops, the data is destroyed.

---

## Section 3: Optimization & Security

### Q17: How do you reduce Docker image size?
**Answer**:
1. Use lightweight base images (like `alpine` or `-slim`).
2. Implement multi-stage builds.
3. Minimize the number of layers by chaining commands with `&&`.
4. Use `.dockerignore` to exclude unnecessary files.

### Q18: What is a multi-stage build?
**Answer**: A build method that uses multiple temporary images (stages) during the build process, copying only final artifacts to a minimal runtime image, reducing image size.

### Q19: Why should you avoid running containers as root?
**Answer**: If a container is compromised and the attacker escapes to the host system, they will inherit root permissions on the host system, posing a major security risk.

### Q20: What is layer caching?
**Answer**: Docker caches the result of each instruction step in a Dockerfile. If the instruction and preceding files have not changed, Docker reuses the cached layer on subsequent builds, speeding up compile times.
