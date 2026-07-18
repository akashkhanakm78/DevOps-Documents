# Chapter 14 - 16: Swarm, Kubernetes, and Production Deployments

This manual introduces container orchestration platforms (Docker Swarm and Kubernetes) and highlights production deployment strategies.

---

## Chapter 14: Docker Swarm

**Docker Swarm** is Docker's native clustering and orchestration tool. It turns a group of physical or virtual machines running Docker into a single, virtual Docker engine.

### 14.1 Key Architecture Concepts
* **Manager Nodes**: Directs cluster state, schedules services, and processes API requests.
* **Worker Nodes**: Receives and executes tasks dispatched from the manager nodes.
* **Services**: Defines what task to run on the cluster (similar to Compose service definitions).

```
                      [ Client (CLI) ]
                             |
                             v
                  +----------------------+
                  |     Swarm Manager    | (Dispatches tasks)
                  +----------+-----------+
                             |
              +--------------+--------------+
              |                             |
              v                             v
     +-----------------+           +-----------------+
     |  Worker Node 1  |           |  Worker Node 2  |
     |  (Runs Task A)  |           |  (Runs Task B)  |
     +-----------------+           +-----------------+
```

### 14.2 Essential Swarm Commands
```bash
# Initialize Swarm manager
docker swarm init --advertise-addr <manager-ip>

# Join cluster as a worker (run command outputted by init on workers)
docker swarm join --token <token> <manager-ip>:2377

# Deploy a replicated service
docker service create --replicas 3 -p 80:80 --name web-service nginx:alpine

# Scale the service dynamically
docker service scale web-service=5

# Inspect running services
docker service ls
```

---

## Chapter 15: Introduction to Kubernetes

**Kubernetes (K8s)** is a powerful, production-grade container orchestration system originally developed by Google. While Swarm is simple and easy to configure, Kubernetes is highly customizable, resilient, and the industry standard for large enterprise applications.

### 15.1 Comparison Table

| Feature | Docker Swarm | Kubernetes (K8s) |
| :--- | :--- | :--- |
| **Complexity** | Low (uses standard Docker CLI tools). | High (requires learning YAML schemas, `kubectl`, and concepts). |
| **Auto-scaling** | Manual scaling commands only. | Automatic scaling based on CPU/RAM metrics (HPA). |
| **Fault Tolerance** | Automatic container restarts on node failure. | Advanced self-healing, auto-restarts, readiness probes. |
| **Networking** | Built-in ingress overlay network. | Flat networking model requiring CNI plugins (Calico, Flannel). |

---

## Chapter 16: Production Deployment Best Practices

Deploying Docker containers in enterprise settings requires specialized configuration:

### 16.1 Logging and Log Rotation
Containers that output logs directly to stdout/stderr can quickly exhaust host storage if left unchecked.

#### Configure Log Rotation in `/etc/docker/daemon.json`
```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```
* **Explanation**: Restricts log file storage per container to a maximum of 3 logs of 10MB each.

### 16.2 Restart Policies
Ensure containers recover gracefully from system crashes or failures:
* `no`: Do not restart automatically (default).
* `on-failure`: Restart if the container exits due to an error (non-zero exit code).
* `always`: Always restart the container if it stops.
* `unless-stopped`: Always restart unless explicitly stopped by the user.

```bash
# Restart container automatically unless stopped manually
docker run -d --restart unless-stopped nginx
```
