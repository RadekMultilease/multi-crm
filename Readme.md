# Multi-CRM Infrastructure (GitOps with Flux CD)

This repository contains the infrastructure and application definitions for the Multi-CRM system. The project follows a **GitOps** methodology using **Flux CD**, ensuring the Kubernetes cluster state is automatically synchronized with this Git configuration.

## 🏗️ Repository Structure

The project is organized into reusable Helm charts and environment-specific markers:

* `./main-helm/` – A generic Helm chart used for application components (API, Frontend, Zarządca). It defines standard Deployments, Services, and Resource limits.
* `./infrastructure-helm/` – Helm charts for core infrastructure components, specifically **PostgreSQL** using StatefulSets.
* `./clusters/` – A marker directory for Flux implementations. Actual cluster configurations (Win-Minikube, Pi-Office) are maintained as separate Flux sources to keep this repo clean.

## 🚀 Deployment Workflow

This repository acts as the **Global Source of Truth**. Environment-specific parameters (replica counts, image tags, storage sizes) are managed via `HelmRelease` manifests in the respective cluster repositories.

### Key Components:
1.  **PostgreSQL**: Deployed via `StatefulSet` with Persistent Volume Claims (PVC) to ensure data persistence across restarts.
2.  **Application Layer**: Deployed as microservices using the `main-helm` template, allowing for consistent configuration across different environments.

## 🔐 Security & Secrets

**Important:** Secret files (`secrets.yaml`) are strictly ignored by Git via `.gitignore`. 
Before Flux can successfully deploy the database or applications on a new cluster, you must manually apply the required credentials:

```bash
# Apply the database credentials once per cluster
kubectl apply -f infrastructure-helm/secrets.yaml