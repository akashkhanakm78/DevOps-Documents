# Chapter 03: Kubernetes Networking & Services

This manual explains how Kubernetes manages pod-to-pod communication, cluster-internal access, public load balancing, and Ingress routing rules.

---

## 1. The Kubernetes Networking Model

In Kubernetes:
* **Unique IP per Pod**: Every Pod in a cluster receives a unique IP address. Containers inside the same Pod share the same network namespace and IP address (communicating via `localhost`).
* **Ephemeral Sockets**: Pods are transient. If a Pod crashes and is recreated by a Deployment, it receives a new IP address. 
* **The Problem**: You cannot configure backend target IPs directly in your frontend code because backend Pod IPs change constantly.
* **The Solution**: **Services**. A Service is an abstract way to expose an application running on a set of Pods as a network service, providing a single, stable IP address and DNS name.

```
                     [ FRONTEND POD ]
                            |
                            v (Connects to stable service DNS)
              [ backend-service: 10.96.0.45 ] (Stable IP)
                            |
            +---------------+---------------+
            | (Load Balances Traffic)       |
            v                               v
    [ BACKEND POD 1 ]               [ BACKEND POD 2 ]
      IP: 10.244.1.5                  IP: 10.244.2.8
```

---

## 2. Service Types

Kubernetes offers different Service types to expose workloads:

### 2.1 ClusterIP (Default)
Exposes the Service on a cluster-internal IP. 
* **Access**: Accessible only from within the cluster.
* **Use Case**: Databases or internal APIs that should not be exposed to the internet.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: db-service
spec:
  type: ClusterIP
  selector:
    app: mongodb # Route traffic to Pods with this label
  ports:
    - port: 27017 # Port exposed by the Service
      targetPort: 27017 # Port the container is listening on
```

### 2.2 NodePort
Exposes the Service on each Node’s IP at a static port (between 30000-32767).
* **Access**: Accessible from outside the cluster by requesting `<NodeIP>:<NodePort>`.
* **Use Case**: Basic test setups.

### 2.3 LoadBalancer
Exposes the Service externally using a cloud provider’s load balancer.
* **Access**: Accessible publicly via the generated load balancer DNS.
* **Use Case**: Exposing frontends or public-facing gateways (like AWS EKS integration, which automatically provisions an AWS Classic or Network Load Balancer).

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  type: LoadBalancer
  selector:
    app: web-pod
  ports:
    - port: 80
      targetPort: 80
```
* **EKS Integration**: When you apply this manifest on AWS EKS, AWS automatically provisions an AWS Elastic Load Balancer (ELB) in your VPC and routes incoming public port 80 traffic to your worker node instances.
