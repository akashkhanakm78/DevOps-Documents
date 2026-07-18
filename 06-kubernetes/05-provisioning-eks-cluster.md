# Chapter 05: Provisioning AWS EKS Clusters

This manual provides the step-by-step installation guides for command-line tools and how to provision a production-ready AWS EKS cluster.

---

## 1. Prerequisites Tooling Installation

Before provisioning, you must install three essential command-line utilities on your administrative machine:

### 1.1 AWS CLI (AWS Command Line Interface)
* **Purpose**: Authenticates with your AWS account and executes API actions.
* **Setup**: Download the installer from the [AWS CLI Official Guide](https://aws.amazon.com/cli/).
* **Configure**: Run `aws configure` and enter your IAM Access Key, Secret Key, default region (e.g. `us-east-1`), and output format (`json`).

### 1.2 kubectl (Kubernetes Control Tool)
* **Purpose**: CLI interface used to deploy apps, inspect resources, and view cluster status.
* **Install (Linux)**:
  ```bash
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  ```

### 1.3 eksctl (The official CLI for Amazon EKS)
* **Purpose**: A simple command-line utility for creating and managing clusters on EKS.
* **Install (Linux)**:
  ```bash
  curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
  sudo mv /tmp/eksctl /usr/local/bin
  ```

---

## 2. Provisioning EKS Cluster via `eksctl`

The easiest way to provision a cluster is using `eksctl`. It automatically creates a new AWS VPC, subnets, routing tables, and provisions the EC2 Node Group machines.

### 2.1 The Provisioning Command
Run the following command to deploy a cluster:

```bash
eksctl create cluster \
  --name dev-cluster \
  --region us-east-1 \
  --nodegroup-name dev-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --managed
```
* **Parameters**:
  * `--node-type t3.medium`: Specifies the hardware capacity of the worker nodes (2 vCPUs, 4GB RAM).
  * `--nodes 2`: Starts the cluster with 2 worker node instances.
  * `--nodes-min 1 / --nodes-max 3`: Configures auto-scaling boundaries.
  * `--managed`: Provisions an EKS Managed Node Group (AWS manages node OS upgrades).

> [!NOTE]
> EKS cluster creation can take between **10 to 20 minutes** as AWS provisions the multi-AZ control plane databases and sets up network VPC routing.

---

## 3. Configuring kubectl Context

Once `eksctl` finishes cluster creation, it automatically updates your local configuration file (located in `~/.kube/config`). 

To verify connectivity or manually generate the config file:
```bash
# Retrieve connection credentials and update local kubeconfig context
aws eks update-kubeconfig --region us-east-1 --name dev-cluster

# Verify connection to the cluster by listing active nodes
kubectl get nodes
```

### Expected Output
```text
NAME                         STATUS   ROLES    AGE   VERSION
ip-192-168-1-50.ec2.internal Ready    <none>   5m    v1.27.x
ip-192-168-1-51.ec2.internal Ready    <none>   5m    v1.27.x
```
* **STATUS Ready**: Confirms worker nodes have successfully established connection with EKS Control Plane API.
