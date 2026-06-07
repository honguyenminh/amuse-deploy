# amuse-deploy

GitOps **live desired state** for Amuse on Kubernetes (dev K3s + stage AKS).

| Repo | Role |
|------|------|
| [honguyenminh/amuse](https://github.com/honguyenminh/amuse) | Application source + manifest **templates** (`infrastructure/kubernetes/`) |
| **This repo** | What Argo CD syncs to clusters |

## Layout

```
base/              # synced from amuse
overlays/dev/      # K3s dev
overlays/stage/    # AKS staging
argocd/            # Argo CD Application CRs (deploy-repo only)
```

## Deploy-only files (not overwritten by sync from amuse)

- `overlays/*/images-tags/` — image tags; updated by CI (`backend-deploy` workflow in amuse)
- `overlays/*/secrets.yaml` — optional real secrets (gitignored)
- `argocd/` — managed only in this repo

## Argo CD

**K3s (dev):**

```bash
kubectl apply -f argocd/projects/amuse-project.yaml
kubectl apply -f argocd/bootstrap/dev-application.yaml
```

**AKS (stage):**

```bash
kubectl apply -f argocd/projects/amuse-project.yaml
kubectl apply -f argocd/bootstrap/stage-application.yaml
```

## Manual validation

```bash
kubectl kustomize overlays/dev
kubectl kustomize overlays/stage
```
