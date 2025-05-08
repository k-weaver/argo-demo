# Description

This is a demo repo for bootstrapping Argo and demonstrating a repo layout that will support GitOps workflows.

## Repo Layout

### Application Source (apps/)

This directory holds the raw application manifests (or Helm chart values, etc.). Kustomize overlays can be used to manage environment-specific differences, but this is not a requirement.

### Cluster Configurations (clusters/)

Cluster-level configurations such as namespace creation, RBAC, and other infrastructural settings. This can be used for cluster bootstrapping.

### ArgoCD Application Manifests (argo-apps/)

In each environment’s folder (dev, staging, prod), you define a set of ArgoCD Application CRs that point to the relevant directories in your apps/ folder. The “App of Apps” pattern is used here:

    • app-of-apps.yaml: Acts as the root for that environment’s applications. When you deploy this Application to ArgoCD, it will discover and deploy the child Application CRs defined in the same folder.

    • Individual App Manifests: Each file (like external-dns.yaml or my-app.yaml) defines an ArgoCD Application resource that specifies the repository, the target path (e.g., apps/external-dns/dev), and the destination (cluster/namespace).

## Quick and Dirty Install

Repo credentials (k8s secret) needs to exist prior to running the bootstrap. There is also a race condition when bootstrap.yaml is included in the kustomization.yaml file where the projects attempt to create before the CRDs are fully up; This is why they have been separated.

    ```bash
    kubectl apply -k clusters/dev/infrastructure/.
    kubectl apply -f clusters/dev/bootstrap.yaml
    ```
