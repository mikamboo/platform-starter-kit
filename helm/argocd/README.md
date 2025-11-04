# Custom Argo CD Helm Chart Wrapper

> We create a Helm "umbrella chart". This is basically a custom chart that wraps another chart. It pulls the original chart in as a dependency, and overrides the default values. Using this approach, we have more flexibility in the future, by possibly including additional Kubernetes resources. 

https://www.arthurkoziel.com/setting-up-argocd-with-helm

## Generate the helm chart lock file

```bash
helm repo add argo-cd https://argoproj.github.io/argo-helm
helm dependency update helm/argocd
```

## Install ArgoCD using the custom Helm chart

```bash
helm install argo-cd helm/argocd \
  --namespace argocd \
  --create-namespace \
  --values helm/argocd/values.yaml
```