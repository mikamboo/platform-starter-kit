## GitOps Platform with Argo + Crossplane

Platform Engineering Starter Kit Series: GitOps Bootstrap with Argo CD (3)

* https://jiminbyun.medium.com/platform-engineering-starter-kit-series-gitops-bootstrap-with-argo-cd-3-656db9ebec95

Prepare helm

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

### Install crossplane

```bash
helm upgrade --install crossplane crossplane-stable/crossplane \
  --namespace crossplane-system \
  --version 2.0.2 \
  -f helm/crossplane/values.yaml \
  --create-namespace
```

### Create repo secret (private repository access)

When App of Apps pattern is used, ArgoCD needs to access the Git repository where the application manifests are stored. Create a secret that contains the repository credentials.

```bash
kubectl apply -f argocd/repositories.yaml
```

### Install ArgoCD root application

```bash
kubectl apply -f argocd/root-app.yaml
```