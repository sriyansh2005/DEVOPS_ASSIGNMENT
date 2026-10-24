# Exercise 2 — Deploying a Custom Flask App on Kubernetes

Build a Flask app into a Docker image, load it into Minikube, and run it on Kubernetes with a Deployment + NodePort Service.

## Files

| File | Purpose |
|------|---------|
| `app.py` | Flask app, serves `Hello from Flask on Kubernetes!` on port `15000` |
| `Dockerfile` | Builds the `flask-app` image on `python:3.11-slim` |
| `flask-deployment.yaml` | Deployment (1 replica) + NodePort Service |

## Steps

```bash
# Build the image inside Minikube's Docker daemon
eval $(minikube docker-env)
docker build -t flask-app:latest .

# (alternative) build locally, then load into Minikube
# docker build -t flask-app:latest .
# minikube image load flask-app:latest

# Deploy to Kubernetes
kubectl apply -f flask-deployment.yaml

# Verify
kubectl get pods
kubectl get deployments
kubectl get services

# Get the service URL (keep this terminal open on Docker driver / macOS)
minikube service flask-app-service --url
```

The manifest uses `imagePullPolicy: Never` so Kubernetes uses the locally built image instead of pulling from a registry.

## Result

```bash
$ curl http://127.0.0.1:52041
Hello from Flask on Kubernetes!
```

| Resource | Value |
|----------|-------|
| Deployment | `flask-app` (1/1 ready) |
| Service type | NodePort |
| Port mapping | 15000 → 31487 |

> On the Docker driver on macOS, access the app through the `minikube service … --url` tunnel — keep that terminal open while it runs.
