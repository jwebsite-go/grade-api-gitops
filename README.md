# grade-api-gitops

GitOps repository for deploying **grade-submission-api** to Kubernetes using ArgoCD.

## How It Works

Every push to `main` automatically syncs the deployment to the Kubernetes cluster via ArgoCD.

```
GitHub (main) → ArgoCD (auto-sync) → Kubernetes (grade namespace)
```

## Project Structure

```
.
└── deployment.yaml          # Kubernetes Deployment + Service
```

## Resources

| Resource   | Details                          |
|------------|----------------------------------|
| Deployment | `grade-submission-api`           |
| Namespace  | `grade`                          |
| Replicas   | 3                                |
| Port       | 3000 (exposed via Service on 80) |
| Image      | `ghcr.io/jumptotechschooldevops/k8s-ci-build` |

## Prerequisites

- Kubernetes cluster
- ArgoCD installed in `argocd` namespace
- Access to the container registry (`ghcr-secret`)

## Setup

1. Create the namespace:
```bash
kubectl create namespace grade
```

2. Apply the deployment:
```bash
kubectl apply -f deployment.yaml
```

3. Verify pods are running:
```bash
kubectl -n grade get pods
```

## ArgoCD

The ArgoCD application `grade-api` is configured with auto-sync enabled (`prune` and `selfHeal`). Any changes pushed to `main` will be automatically applied to the cluster.

To check the sync status:
```bash
kubectl -n argocd get app grade-api
```

To access the ArgoCD UI locally:
```bash
kubectl -n argocd port-forward svc/argocd-server 8443:443
```
Then open `https://localhost:8443` in the browser.
