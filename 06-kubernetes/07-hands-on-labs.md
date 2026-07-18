# Chapter 07: Kubernetes Hands-on Labs

Follow this lab exercise step-by-step to deploy a multi-tier web application (Nginx frontend and Node.js backend API) on AWS EKS.

---

## Lab: Deploy a Multi-Tier Application on AWS EKS

**Goal**: Deploy an application containing a backend API service and a frontend web interface. The frontend will be exposed publicly using an AWS Load Balancer.

---

### Step 1: Deploy Backend API

Create a file named `backend-deploy.yaml` to run a Node.js API server:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: api-pod
  template:
    metadata:
      labels:
        app: api-pod
    spec:
      containers:
        - name: node-api
          image: node:18-alpine
          command: ["node", "-e", "const http = require('http'); http.createServer((req, res) => res.end('API Response')).listen(3000)"]
          ports:
            - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: api-service # Exposed internally as db-service/api-service DNS
spec:
  type: ClusterIP # Internal only
  selector:
    app: api-pod
  ports:
    - port: 3000
      targetPort: 3000
```
Deploy the backend:
```bash
kubectl apply -f backend-deploy.yaml
```

---

### Step 2: Deploy Frontend Web Interface

Create a file named `frontend-deploy.yaml` running Nginx:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web-pod
  template:
    metadata:
      labels:
        app: web-pod
    spec:
      containers:
        - name: nginx-web
          image: nginx:alpine
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  type: LoadBalancer # Automatically provisions AWS ELB Load Balancer
  selector:
    app: web-pod
  ports:
    - port: 80
      targetPort: 80
```
Deploy the frontend:
```bash
kubectl apply -f frontend-deploy.yaml
```

---

### Step 3: Verify Deployments and Load Balancer Routing

Inspect the state of your cluster:
```bash
# Get running Pods list
kubectl get pods

# Get active services
kubectl get svc
```

### Expected Output for `get svc`
```text
NAME          TYPE           CLUSTER-IP      EXTERNAL-IP                                 PORT(S)        AGE
api-service   ClusterIP      10.100.20.10    <none>                                      3000/TCP       2m
web-service   LoadBalancer   10.100.50.80    a1b2c3d4-elb.us-east-1.amazonaws.com        80:31256/TCP   2m
```
* **EXTERNAL-IP**: Shows the DNS endpoint for the newly provisioned AWS Elastic Load Balancer (ELB).
* **Test access**: Copy the `EXTERNAL-IP` address (e.g., `a1b2c3d4-elb.us-east-1.amazonaws.com`) and paste it into your browser. You will see Nginx's welcome page!

---

### Step 4: Clean up AWS Resources
To avoid incurring AWS charges for the Load Balancer and EKS cluster when you finish:
```bash
# 1. Delete the Kubernetes services (this releases the AWS ELB)
kubectl delete -f frontend-deploy.yaml
kubectl delete -f backend-deploy.yaml

# 2. Terminate EKS cluster and VPC network configurations
eksctl delete cluster --name dev-cluster --region us-east-1
```
* **Note**: Always delete LoadBalancer services *before* deleting the cluster to ensure AWS deletes the Load Balancers cleanly.
