## GitOps Platform with Argo + Crossplane

Platform Engineering Starter Kit Series: GitOps Bootstrap with Argo CD (3)

* https://jiminbyun.medium.com/platform-engineering-starter-kit-series-gitops-bootstrap-with-argo-cd-3-656db9ebec95


### Prepare installation

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

### Bootstrap argocd installation

Installation steps:

```bash
# 1. Install ArgoCD first
helm install argo helm/argocd \
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

### Access ArgoCD UI

```bash
# Port-forward ArgoCD server to localhost
kubectl port-forward svc/argo-argocd-server -n argocd 8080:443

# Get initial admin password
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d; echo
```

### TODO

- [x] Let ArgoCD manage itself
- [x] Install Crossplane via ArgoCD
- [-] Create root app Helm chart
- [x] Let ArgoCD manage itself via root app
- [-] Add observability stack (Prometheus + Grafana + Loki) via ArgoCD
- [ ] Create EKS cluster and deploy the kit
- [ ] Put all apps as root app chart templates
- [ ] Setup Crossplane provider for AWS
- [ ] Setup SSO login for ArgoCD

### Layers of bootstrapping

Layer 0. Base platform infrastructure
  - AWS account setup (IAM roles, OIDC provider, etc.)
  - EKS cluster provisioning

Layer 1. Base platform components
  - ArgoCD setup
  - Crossplane setup
  - Observability setup

Layer 2. Platform shared services
  - SSO integration
  - Team namespaces provisioning
