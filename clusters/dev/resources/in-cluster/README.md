# Cluster Resources

This directory contains all cluster resources that should be applied to cluster: `in-cluster`.
For example `Namespace` resources that are shared by multiple applications on the same namespace.

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl apply -f /Users/kacee.weaver/src/projects/kind/argo-demo/clusters/dev/bootstrap.yaml
```
