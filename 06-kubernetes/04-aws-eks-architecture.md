# Chapter 04: AWS EKS Architecture & IAM Roles

This manual covers AWS Elastic Kubernetes Service (EKS) architecture, compute node choices, and IAM integrations.

---

## 1. What is AWS EKS?

**AWS Elastic Kubernetes Service (EKS)** is a managed service that makes it easy to run Kubernetes on AWS without needing to install, operate, and maintain your own Kubernetes control plane.

```
       [ AWS EKS Managed Control Plane ] (Cross-AZ, Highly Available)
       - API Server, etcd database, scheduler, controllers
                              |
                              | Connects to Node Groups
                              v
       +-------------------------------------------------------------+
       |                  CUSTOMER VPC / WORKER NODES                |
       |                                                             |
       |  +-----------------------+       +-----------------------+  |
       |  | Managed Node Group    |       | AWS Fargate           |  |
       |  | (EC2 Instances)       |  OR   | (Serverless compute)  |  |
       |  +-----------------------+       +-----------------------+  |
       +-------------------------------------------------------------+
```

### 1.1 High Availability by Default
AWS provisions and manages the Control Plane across multiple AWS Availability Zones (AZs) automatically, providing scalability and failover reliability. AWS is responsible for backing up the etcd database, patching security fixes, and scaling Control Plane APIs.

---

## 2. EKS Compute Options

When configuring worker nodes (data plane) in EKS, you have two choices:

### 2.1 Managed Node Groups (EC2)
* **What it is**: AWS provisions and manages Amazon EC2 instances as worker nodes in your account.
* **Management**: You specify the instance types (e.g., `t3.medium`, `m5.large`) and scaling limits (min/max nodes). AWS manages OS upgrades and patches.
* **Billing**: You pay for the EC2 instances and EBS volumes provisioned.
* **Control**: Complete access to SSH into nodes.

### 2.2 AWS Fargate (Serverless)
* **What it is**: Run Kubernetes Pods without provisioning or managing EC2 server instances.
* **Management**: You define a Fargate profile matching your namespace. Kubernetes schedules Pods directly onto Fargate.
* **Billing**: You are billed per second based on the exact vCPU and Memory allocated to each active Pod container.
* **Control**: No server instances to manage, patch, or SSH into.

---

## 3. IAM Roles for Service Accounts (IRSA)

In standard setups, if a Pod container needs to access an AWS resource (like reading files from an S3 bucket or writing data to DynamoDB), it requires AWS credentials.
* **The Old Way**: Assigning administrative IAM roles to the worker node EC2 instance directly. This is a security risk because *any* Pod on that node inherits full access.
* **The EKS Way (IRSA)**: Integrates AWS Identity and Access Management (IAM) with Kubernetes Service Accounts.
  * EKS establishes an **OpenID Connect (OIDC)** provider link.
  * You create an IAM role with specific, narrow permissions (e.g., read-only access to a specific S3 bucket).
  * You annotate a Kubernetes Service Account with the IAM role ARN.
  * EKS automatically injects temporary AWS security tokens into Pod containers that use that Service Account.
