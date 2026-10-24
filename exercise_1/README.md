# Exercise 1 — Deploying nginx on Kubernetes (Minikube)

Run an `nginx` container as a pod on a local Minikube cluster and access it in the browser via a NodePort service.

## Prerequisites

- Docker
- Minikube
- kubectl

## Steps

```bash
# Start the cluster (Docker driver)
minikube start

# Verify the control-plane and system pods are up
kubectl get nodes
kubectl get pods -A

# Run nginx as a pod
kubectl run hello-k8s --image=nginx --port=80

# Expose it as a NodePort service
kubectl expose pod hello-k8s --type=NodePort --port=80

# Check pod and service
kubectl get pods
kubectl get services

# Open the service in the browser (Docker driver on macOS needs this tunnel)
minikube service hello-k8s
```

## Result

`kubectl get services` shows the NodePort mapping (`80:31423/TCP`), and `minikube service hello-k8s` opens a tunnel serving the default **Welcome to nginx!** page.

| Resource | Value |
|----------|-------|
| Pod | `hello-k8s` (1/1 Running) |
| Service type | NodePort |
| Cluster IP | 10.99.200.96 |
| Port mapping | 80 → 31423 |

> On the Docker driver on macOS, keep the `minikube service` terminal open — it holds the tunnel that makes the service reachable.
