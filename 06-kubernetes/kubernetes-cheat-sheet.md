# Kubernetes kubectl Cheat Sheet

Quick command reference sheet for the Kubernetes CLI utility `kubectl`.

---

## 1. Inspecting Cluster State
```bash
kubectl get nodes                         # List all active worker nodes
kubectl get pods                          # List pods in the active namespace
kubectl get pods -o wide                  # List pods with IPs and hosting Node names
kubectl get services                      # List active services and IPs
kubectl get deployments                   # List deployments and replica status
kubectl get namespaces                    # List all logical namespaces
```

## 2. Creating & Applying Resources
```bash
kubectl apply -f manifest.yaml            # Create or update resources defined in a file
kubectl delete -f manifest.yaml           # Delete resources defined in a file
kubectl delete pod my-pod                 # Delete a specific Pod
```

## 3. Diagnostics & Troubleshooting
```bash
kubectl describe pod my-pod               # Inspect detailed events & configurations of a Pod
kubectl logs my-pod                       # View stdout logs of a container
kubectl logs my-pod -c my-container       # View logs of a specific container inside a multi-container Pod
kubectl logs -f my-pod                    # Stream logs in real-time
kubectl logs my-pod --previous            # View logs of a crashed container instance
kubectl exec -it my-pod -- sh             # Open an interactive shell inside a running container
```

## 4. Scaling & Rollouts
```bash
kubectl scale deployment web-deploy --replicas=5 # Scale replicas to 5 dynamically
kubectl rollout status deployment web-deploy     # View active status of deployment rollout updates
kubectl rollout undo deployment web-deploy       # Rollback deployment to the previous version
kubectl rollout history deployment web-deploy    # View revision history of deployment rollouts
```

## 5. EKS Management Context
```bash
# Update local kubeconfig to point to AWS EKS cluster
aws eks update-kubeconfig --region us-east-1 --name dev-cluster
```
