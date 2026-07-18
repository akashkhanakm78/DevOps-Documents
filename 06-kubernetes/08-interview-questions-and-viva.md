# Chapter 08: Kubernetes & AWS EKS Interview Prep

A compilation of the most frequently asked Kubernetes and AWS EKS administration interview questions.

---

## Section 1: Kubernetes Core Architecture

### Q1: What is a Pod in Kubernetes?
**Answer**: A Pod is the smallest deployable unit you can create and manage in Kubernetes. It represents a single instance of a running process in your cluster. A Pod can host one or more containers that share the same network namespace (IP address and ports) and storage volumes.

### Q2: What is the difference between a Deployment and a StatefulSet?
**Answer**:
* **Deployment**: Used for stateless applications (like web frontends or APIs). Pods are identical, interchangeable, and receive random hostnames. If a Pod is replaced, it does not preserve data or identity.
* **StatefulSet**: Used for stateful applications (like databases or Kafka brokers). Pods have persistent, unique, ordinal identifiers (e.g., `db-0`, `db-1`) and are matched to persistent storage volumes that remain bound to the Pod identity even if the Pod is rescheduled to another node.

### Q3: What is `etcd` in the Control Plane?
**Answer**: `etcd` is a highly available, distributed, key-value data store used as the backing database for Kubernetes. It stores the complete configuration state, specifications, and status of all resources in the cluster. It is the single source of truth for cluster state.

---

## Section 2: AWS EKS Integration

### Q4: Explain the difference between EKS Managed Node Groups and AWS Fargate.
**Answer**:
* **Managed Node Groups**: Run Pods on Amazon EC2 instances in your account. You manage instance sizing and scaling. You pay for the EC2 capacity, and you have complete SSH root access to nodes.
* **AWS Fargate**: Runs Pods on a serverless compute engine. AWS manages node infrastructure, OS patching, and capacity scaling. You are billed based on the exact vCPU and memory allocated to your running Pods, and you cannot SSH into server instances.

### Q5: What is IAM Roles for Service Accounts (IRSA), and why is it used?
**Answer**: IRSA is a security feature that allows you to assign specific AWS IAM permissions directly to a Kubernetes Service Account instead of assigning them to the hosting EC2 instance nodes. It utilizes an OpenID Connect (OIDC) federation link. It follows the principle of least privilege, ensuring that only Pods that require access (e.g. database backups to S3) receive AWS credentials.

---

## Section 3: Diagnostic Scenarios

### Q6: How do you troubleshoot a Pod that is stuck in the `ImagePullBackOff` state?
**Answer**:
1. Check the Pod events: `kubectl describe pod <pod-name>`.
2. Common causes:
   * The container image name or tag is misspelled in the YAML manifest.
   * The image does not exist on the registry (Docker Hub/AWS ECR).
   * The registry is private, and the cluster lacks the necessary credentials (pull secrets).

### Q7: What does the `CrashLoopBackOff` status mean, and how do you debug it?
**Answer**: It means that the Pod container was successfully pulled and started, but the process inside exited or crashed immediately. Kubernetes attempts to restart the container, but enters a loop where it continues crashing.
* **How to debug**:
  1. Read the application logs: `kubectl logs <pod-name>`.
  2. If the container crashed before logging, check logs of the previous instance: `kubectl logs <pod-name> --previous`.
  3. Inspect events: `kubectl describe pod <pod-name>`. Common causes include missing environment variables, database connection failures, or runtime syntax errors.
