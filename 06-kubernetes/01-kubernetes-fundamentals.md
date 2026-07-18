# Chapter 01: Kubernetes Fundamentals

This manual introduces Kubernetes, explaining the need for container orchestration, and details the Control Plane and Worker Node architectures.

---

## 1. What is Kubernetes?

**Kubernetes** (commonly abbreviated as **K8s**, representing the 8 letters between "K" and "s") is an open-source system for automating deployment, scaling, and management of containerized applications. It was originally designed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

### 1.1 Why Do We Need Container Orchestration?
While running containers using `docker run` or `docker compose` is fine for local development, managing containers at enterprise scale in production introduces challenges:
* **Self-Healing**: If a container host physical server crashes, how do we auto-restart containers on another machine?
* **Auto-Scaling**: If traffic spikes, how do we spin up more container instances automatically?
* **Load Balancing**: How do we distribute traffic across multiple container instances?
* **Zero-Downtime Rollouts**: How do we deploy new code updates without causing service outages?

Kubernetes solves all of these issues by managing a cluster of host virtual machines as a single unified computing pool.

---

## 2. Kubernetes Architecture

A Kubernetes cluster is divided into two primary sections: the **Control Plane** (Master Nodes) and the **Worker Nodes**.

```
+--------------------------------------------------------------------------+
|                              CONTROL PLANE                               |
|                                                                          |
|  +------------------+     +------------------+     +------------------+  |
|  |    API Server    | <== |    Scheduler     |     | Controller Mgr   |  |
|  +--------+---------+     +------------------+     +------------------+  |
|           |                                                              |
|           v                                                              |
|       +---+---+                                                          |
|       | etcd  | (Key-value state store)                                  |
|       +-------+                                                          |
+-----------|--------------------------------------------------------------+
            |
            | (Internal Communication)
            v
+-----------|--------------------------------------------------------------+
|           |                   WORKER NODE 1                              |
|           v                                                              |
|  +------------------+     +------------------+     +------------------+  |
|  |     Kubelet      |     |    Kube-Proxy    |     |  Container Run   |  |
|  +------------------+     +------------------+     |  (Containerd)    |  |
|                                                    +---------+--------+  |
|                                                              |           |
|                                                              v           |
|                                                        [  PODS  ]        |
+--------------------------------------------------------------------------+
```

---

## 3. Control Plane Components (The Brain)

The Control Plane makes global decisions about the cluster (such as scheduling workloads) and detects and responds to cluster events.

1. **kube-apiserver**: The entry point for the cluster. It exposes the Kubernetes API and receives commands from users (via `kubectl` CLI) and internal components.
2. **etcd**: A consistent and highly-available key-value store used as Kubernetes' backing database for all cluster data and state configurations.
3. **kube-scheduler**: Watches for newly created Pods that have no assigned node, and selects a healthy worker node for them to run on (based on resource requirements and constraints).
4. **kube-controller-manager**: Runs controller processes that regulate the state of the cluster. Examples:
   * **Node Controller**: Detects when worker nodes crash.
   * **Replication Controller**: Ensures the correct number of Pod instances are running.

---

## 4. Worker Node Components (The Muscle)

Worker Nodes maintain running pods and provide the Kubernetes runtime environment.

1. **Kubelet**: An agent that runs on each node in the cluster. It ensures that containers defined in Pod specifications are running and healthy.
2. **Kube-Proxy**: A network proxy that runs on each node. It maintains network rules on host ports to allow network communication to Pods from inside or outside the cluster.
3. **Container Runtime**: The software responsible for running containers (e.g., `containerd` or Docker Engine).
4. **Pod**: The smallest deployable computing unit you can create and manage in Kubernetes. A Pod hosts one or more containers (sharing storage and network interfaces).
