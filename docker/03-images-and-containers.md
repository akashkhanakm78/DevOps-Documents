# Chapter 06 - 07: Images, Layer Caching, and Container Lifecycle

This manual explores the structure and mechanics of Docker Images and explains the complete Lifecycle of Docker Containers.

---

## Chapter 06: Understanding Docker Images

A **Docker Image** is a read-only, multi-layered template containing the application code, libraries, dependencies, configuration files, and runtime environment.

### 6.1 The Union File System (UnionFS)
Docker images are built as a stack of layers. Each instruction in a Dockerfile adds a new layer.
* **Layers are Read-Only**: Once created, image layers are immutable and read-only.
* **Shared Storage**: If two images share the same base layer (e.g., `ubuntu:22.04`), they only store one copy of that layer on the host disk, optimizing storage efficiency.

```
       +---------------------------------------------+
       |   Writable Container Layer (Read-Write)     |  <-- Created when container starts
       +---------------------------------------------+
       |   Layer 3: Application Code (Read-Only)     |
       +---------------------------------------------+
       |   Layer 2: Installed Packages (Read-Only)   |
       +---------------------------------------------+
       |   Layer 1: Base Operating System (Read-Only)|
       +---------------------------------------------+
```

### 6.2 The Writable Container Layer
When you run a container, Docker adds a thin **read-write layer** (also known as the "Container Layer") on top of the stack of read-only image layers.
* All changes made to the running container (such as writing new files or modifying existing ones) are saved in this writable layer.
* When the container is deleted, this writable layer is destroyed, while the underlying image remains unmodified.

### 6.3 Copy-on-Write (CoW) Strategy
If a container needs to modify a file that exists in the read-only image layers:
1. Docker copies the file from the read-only layer up to the writable container layer.
2. The container then modifies the copy in the writable layer.
3. The original file in the read-only layer remains unchanged.

---

## Chapter 07: Container Lifecycle

A Docker container has a lifecycle that transitions through several states: **Created, Running, Paused, Stopped, and Exited**.

```
                   docker create
  [ New Image ] =================> [ Created ]
                                      ||
                         docker start || docker run
                                      \/
                     +--------> [ Running ] <--------+
                     |            ||    ^            |
        docker pause |            ||    |            | docker resume
                     |            \/    ||           |
                     +--------> [ Paused ]           |
                                  ||                 |
                     docker stop  ||                 |
                                  \/                 |
         [ Exited ] <========== [ Stopped ] =========+
             ||
             || docker rm
             \/
        [ Destroyed ]
```

### 7.1 Lifecycle Transitions

1. **Created**: The container is prepared with its read-write layer and configured settings, but its main process is not yet started.
   * Command: `docker create <image-name>`
2. **Running**: The container process is active and running.
   * Command: `docker start <container-id>` or `docker run <image-name>` (creates and starts in one step).
3. **Paused**: The execution of all processes inside the container is suspended.
   * Command: `docker pause <container-id>` (resumed with `docker unpause`).
4. **Stopped**: The container's main process is gracefully terminated (`SIGTERM` followed by `SIGKILL` if it doesn't stop in 10 seconds).
   * Command: `docker stop <container-id>`
5. **Exited**: The container process has completed its execution or crashed. It still occupies disk space until removed.
6. **Destroyed**: The container and its writable layers are completely removed from the host disk.
   * Command: `docker rm <container-id>`

---

## Practical Review Questions

### Q1: What is the Copy-on-Write (CoW) strategy in Docker?
**Answer**: CoW is an optimization strategy where Docker copies a file from the read-only image layer to the writable container layer only when the container needs to write to or modify that file. This saves disk space and speeds up container creation.

### Q2: What is the difference between `docker stop` and `docker kill`?
**Answer**: `docker stop` sends a `SIGTERM` signal to the container's PID 1 process, allowing it to gracefully save state and exit (falling back to `SIGKILL` after a timeout). `docker kill` immediately sends a `SIGKILL` signal to terminate the container processes abruptly.
