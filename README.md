## GitOps Platform with Argo + Crossplane

Platform Engineering Starter Kit Series: GitOps Bootstrap with Argo CD (3)

* https://jiminbyun.medium.com/platform-engineering-starter-kit-series-gitops-bootstrap-with-argo-cd-3-656db9ebec95

Prepare helm

```bash
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

### Bootstrap argocd installation

```bash
helm upgrade --install argocd argo/argo-cd \
  --namespace argocd \
  --version 8.2.1 \
  -f helm/argocd/values.yaml \
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

### TODO

- [x] Let ArgoCD manage itself
- [x] Install Crossplane via ArgoCD
- [-] Create root app Helm chart
- [ ] Add observability stack (Prometheus + Grafana + Loki) via ArgoCD
- [ ] Create EKS cluster and deploy the kit
- [ ] Put all apps as root app chart templates
- [ ] Setup Crossplane provider for AWS
- [ ] Setup SSO login for ArgoCD