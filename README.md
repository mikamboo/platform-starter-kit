## GitOps Platform with Argo + Crossplane

Platform Engineering Starter Kit Series: GitOps Bootstrap with Argo CD (3)

* https://jiminbyun.medium.com/platform-engineering-starter-kit-series-gitops-bootstrap-with-argo-cd-3-656db9ebec95

Prepare helm

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

### Bootstrap argocd installation

Download chart locally then install with custom values.yaml


```bash

# 1. Install ArgoCD first
helm install argo-cd helm/argocd \
  --namespace argocd \
  --create-namespace \
  --values helm/argocd/values.yaml

# 2. Wait for ArgoCD to be ready
kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=argocd-server -n argocd --timeout=300s

# 3. Create repository secrets BEFORE root app
# ArgoCD apps need to access the Git repo where the application manifests are stored. 
# We need to create argocd typed secrets that contains the repository credentials.
kubectl apply -f helm/root-app/templates/repositories.yaml

# 4. Finally create the ArgoCD root app
kubectl apply -f helm/root-app/templates/root-app.yaml
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