# Cluster Configurations

This directory serves as a marker for Kubernetes cluster implementations. 

## ⚠️ Repository Strategy
To maintain a clean separation of concerns, the actual Flux manifests and cluster-specific configurations (HelmReleases, secret references, and gotk-components) are stored in **separate, dedicated repositories** for each environment.

This prevents the "Source of Truth" (the Helm charts in this repo) from being cluttered with environment-specific overrides.

## Current Clusters
* **win-minikube**: [Link to private repo/branch] - Local Windows development environment.

## Deployment Workflow
1. **Charts:** All logic is defined in `/main-helm` and `/infrastructure-helm` in this repo.
2. **Secrets:** Secrets are **not** stored in Git. They must be applied manually to the cluster before Flux syncs:
   `kubectl apply -f infrastructure-helm/secrets.yaml`
3. **Flux Sync:** Each cluster repo points back to this repository as its `GitRepository` source.

---
*Last Updated: January 2026*