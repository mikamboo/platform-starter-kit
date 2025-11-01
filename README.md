Platform Engineering Starter Kit Series: GitOps Bootstrap with Argo CD (3)

https://jiminbyun.medium.com/platform-engineering-starter-kit-series-gitops-bootstrap-with-argo-cd-3-656db9ebec95


Prepare helm repositories

```bash
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

### Install argocd

```bash
helm upgrade --install argocd argo/argo-cd \
  --namespace argocd \
  --version 8.2.1 \
  -f helm/argocd/values.yaml \
  --create-namespace
```

### Create repo secret

Private repositories require authentication. Create them using kubectl.

```yaml

```bash
kubectl apply -f argocd/creds/repo-secret.yaml
```

### Install ArgoCD root application

```bash
kubectl apply -f argocd/apps/root-app.yaml
```