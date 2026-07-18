# Chapter 02: Kubernetes Objects & YAML Manifests

This manual details how to write YAML configurations for Kubernetes objects, including Pods, ReplicaSets, and Deployments.

---

## 1. Structure of a Kubernetes Manifest File

Every Kubernetes object configuration (manifest) written in YAML requires four mandatory root keys:

```yaml
apiVersion: v1             # The API version used to create this object
kind: Pod                  # The type of object (Pod, Service, Deployment, etc.)
metadata:                  # Metadata that uniquely identifies the object
  name: my-web-pod
  labels:
    app: frontend
spec:                      # The desired state configuration for the object
  containers:
    - name: nginx-container
      image: nginx:alpine
      ports:
        - containerPort: 80
```

---

## 2. Pod Manifest Template (`kind: Pod`)

A Pod is the smallest deployable unit in Kubernetes, hosting one or more containers.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: express-api-pod
  labels:
    app: api
    env: production
spec:
  containers:
    - name: express-api
      image: node:18-alpine
      command: ["node", "app.js"]
      ports:
        - containerPort: 3000
      resources: # Configure CPU and Memory limits (Best Practice)
        requests:
          memory: "128Mi"
          cpu: "250m"
        limits:
          memory: "256Mi"
          cpu: "500m"
```
* **`requests`**: The minimum CPU/Memory resources Kubernetes guarantees to allocate to start the container.
* **`limits`**: The maximum CPU/Memory resources the container is allowed to consume. If memory exceeds this limit, the container is OOMKilled (Out Of Memory).

---

## 3. Deployment Manifest Template (`kind: Deployment`)

Deployments manage a set of identical Pods using a **ReplicaSet**. Deployments provide declarative updates for Pods, enabling self-healing, scaling, and zero-downtime rolling updates.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
  labels:
    app: web
spec:
  replicas: 3 # Tells Kubernetes to ensure 3 identical Pods are always running
  selector:
    matchLabels:
      app: web-pod # Links this deployment to the Pod template below
  template: # The Pod blueprint configuration
    metadata:
      labels:
        app: web-pod
    spec:
      containers:
        - name: web-app
          image: nginx:alpine
          ports:
            - containerPort: 80
```

### How Deployments Achieved Self-Healing:
* If a Node hosting one of the Nginx Pods crashes, the Deployment Controller detects that active replicas dropped to 2. It immediately schedules a new Nginx Pod on a remaining healthy Node to restore the replica count back to 3 automatically.
