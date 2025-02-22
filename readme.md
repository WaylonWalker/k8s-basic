# k8s-basic Helm Chart

## Features

## Prerequisites

- **Kubernetes Cluster:** Ensure you have a running Kubernetes cluster.
- **Helm 3:** [Install Helm 3](https://helm.sh/docs/intro/install/).
- **Argo CD (optional):** If you prefer a GitOps approach, install [Argo CD](https://argo-cd.readthedocs.io/en/stable/getting_started/).

## Configuration

The default configuration is defined in `helm-chart/values.yaml`. Here you can set:

``` yaml
```

## Deploying with Argo CD

``` yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: opengraph
  namespace: argocd  # Namespace where Argo CD is installed
spec:
  project: default
  source:
    repoURL: 'https://github.com/waylonwalker/k8s-basic.git'
    targetRevision: HEAD
    path: helm-chart
    helm:
      # valueFiles:
      #   - values.yaml
      # Optional: Override values with parameters
      values: |
        name: opengraph
        port: 8800
        image:
            repository: registry.wayl.one
            name: opengraph
        tag: "0.0.1"
        imagePullSecret: cluster-regcred
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: opengraph  # Target namespace for deployment
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```
