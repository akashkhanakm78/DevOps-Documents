# Chapter 06: Kubernetes Storage and Volumes

This manual explains Persistent Volumes, Persistent Volume Claims, and how to integrate EKS with AWS Elastic Block Store (EBS).

---

## 1. Storage Abstraction in Kubernetes

By default, container filesystems are ephemeral. If a container database crashes and restarts, all written data is lost. To persist files, Kubernetes provides storage abstractions:

* **Persistent Volume (PV)**: A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource.
* **Persistent Volume Claim (PVC)**: A request for storage by a user. It specifies size requirements and access modes (e.g., ReadWriteOnce, ReadOnlyMany).
* **StorageClass (SC)**: Defines the storage type and provisioner. StorageClasses allow dynamic volume provisioning, meaning AWS EKS can automatically create an AWS EBS volume when a developer requests a PVC.

```
  [ Pod Manifest ] ===> [ Persistent Volume Claim (PVC) ] ===> [ StorageClass ]
                                                                      |
                                                                      v (Dynamic Provisioning)
                                                           [ AWS EBS Volume Created ]
```

---

## 2. Dynamic Volume Provisioning on AWS EKS

To enable EKS to automatically provision AWS EBS volumes, you must install the **AWS EBS CSI (Container Storage Interface) Driver**.

### Step 1: Install the EBS CSI Driver
Ensure your worker nodes have IAM permissions to manage EBS volumes, then add the CSI driver:
```bash
# Add the Helm repo or use eksctl to enable the driver addon
eksctl create addon \
  --name aws-ebs-csi-driver \
  --cluster dev-cluster \
  --force
```

### Step 2: Create a StorageClass Manifest (`storage-class.yaml`)
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ebs-sc
provisioner: ebs.csi.aws.com # Use the AWS EBS CSI Driver
volumeBindingMode: WaitForFirstConsumer # Wait until a Pod is scheduled to create the volume
```

### Step 3: Create a Persistent Volume Claim (`pvc.yaml`)
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-pvc
spec:
  accessModes:
    - ReadWriteOnce # Mountable as read-write by a single node
  storageClassName: ebs-sc # Reference the StorageClass defined above
  resources:
    requests:
      storage: 10Gi # Request 10 Gigabytes of storage
```

---

## 3. Mounting the Volume to a Pod

Once the PVC is created, developers can reference and mount the volume inside the Pod container spec:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: database-pod
spec:
  containers:
    - name: mysql
      image: mysql:8.0
      env:
        - name: MYSQL_ROOT_PASSWORD
          value: "password"
      volumeMounts:
        - name: db-storage
          mountPath: /var/lib/mysql # Directory inside container where database saves files
  volumes:
    - name: db-storage
      persistentVolumeClaim:
        claimName: db-pvc # Link to the PVC definition
```
* **Explanation**: When this Pod is scheduled, AWS EKS automatically provisions a physical 10GB AWS EBS volume, mounts it to the hosting EC2 instance, and maps it directly to `/var/lib/mysql` inside the running MySQL container. If the container crashes or gets moved to another node, EKS handles re-mounting the EBS volume to the new node automatically.
