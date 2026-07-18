# Chapter 11 - 12: Networking modes and Volumes

This manual explains how Docker manages container-to-container communication (Networking) and persistent storage (Volumes).

---

## Chapter 11: Docker Networking

Docker assigns containers virtual network adapters and IPs. Docker offers several network drivers that serve different use cases.

### 11.1 Network Drivers

1. **Bridge (Default)**:
   * The default network driver. If you don't specify a driver, this is the type of network you are creating.
   * Containers on the same bridge network can communicate using their container names or IPs (built-in DNS resolution).
2. **Host**:
   * Removes network isolation between the container and the Docker host.
   * The container shares the host’s networking namespace directly (e.g., a container on port 80 will bind directly to the host's port 80 with no port mapping needed).
3. **Overlay**:
   * Connects multiple Docker daemons together and enables Swarm services to communicate with each other across different physical hosts.
4. **Macvlan**:
   * Assigns a physical MAC address to a container, making it appear as a physical device on your network.
5. **None**:
   * Disables all networking for the container. Useful for isolated computation jobs.

```
       +---------------------------------------------+
       |                 DOCKER HOST                 |
       |                                             |
       |  +-------------+           +-------------+  |
       |  | Container A |           | Container B |  |
       |  +------+------+           +------+------+  |
       |         |                         |         |
       |         v                         v         |
       |    [eth0 (172.18.0.2)]       [eth0 (172.18.0.3)]
       |         |                         |         |
       |         +------------+------------+         |
       |                      |                      |
       |                      v                      |
       |            [ Virtual Bridge: docker0 ]      |
       |                      |                      |
       +----------------------|----------------------+
                              v
                       [ Host eth0 IP ]
```

---

## Chapter 12: Storage & Volumes

By default, all files created inside a container are stored on a writable container layer. This means that:
* Data is lost when the container is deleted.
* It is difficult to get the data out of the container if another process needs it.

Docker offers three ways to mount directories to a container: **Volumes, Bind Mounts, and tmpfs mounts**.

```
              [ Host Filesystem ]
              
  +-------------------------------------------------+
  |  /var/lib/docker/volumes/                       |
  |  +-------------------------+                    |
  |  | Docker Managed Volumes  | <=== (Volumes)     |
  |  +-------------------------+                    |
  |                                                 |
  |  /home/user/project/                            |
  |  +-------------------------+                    |
  |  | User-controlled paths   | <=== (Bind Mounts) |
  |  +-------------------------+                    |
  |                                                 |
  |  System RAM / Memory Space                      |
  |  +-------------------------+                    |
  |  | High-speed volatile RAM | <=== (tmpfs)       |
  |  +-------------------------+                    |
  +-------------------------------------------------+
```

### 12.1 Storage Options Compared

| Feature | Docker Volumes | Bind Mounts | tmpfs Mounts |
| :--- | :--- | :--- | :--- |
| **Location on Host**| Managed by Docker (`/var/lib/docker/volumes/` on Linux). | User-specified directory anywhere on the host system. | System memory (RAM) only. Never written to disk. |
| **Portability** | High (backed by volume plugins, Cloud APIs). | Low (relies on host directory layouts and paths). | None (volatile cache only). |
| **Lifecycle** | Persists independently of containers. | Managed manually by the host system user. | Destroyed when container stops. |
| **Safety** | Isolated from host OS corruption. | Containers can modify host OS files (Security Risk). | Safe. |

### 12.2 Usage Commands

#### Mounting a Volume (Recommended)
```bash
# Create a volume
docker volume create my-vol

# Run container with volume mounted
docker run -d --name db -v my-vol:/var/lib/mysql mysql:latest
```

#### Mounting a Bind Mount
```bash
# Mount host directory directly into container (useful for hot-reloading code)
docker run -d --name web -v C:/Users/Mehebub_Alam/Desktop/app:/usr/share/nginx/html nginx:alpine
```
