# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Kubernetes homelab GitOps repository managed by ArgoCD. It contains YAML manifests for deploying and managing applications on a Kubernetes cluster using the GitOps pattern.

## Architecture

The repository follows a two-tier ArgoCD structure:

1. **Bootstrap Layer** (`bootstrap/`): Contains ArgoCD Application manifests that define which applications should be deployed
2. **Application Layer** (`apps/`): Contains the actual Kubernetes manifests or Helm charts for each application

### App-of-Apps Pattern
- `bootstrap/argocd/app-of-apps.yaml` is the root application that manages all other applications
- Each application in `bootstrap/` references its corresponding configuration in `apps/`
- ArgoCD automatically syncs changes from this Git repository to the cluster

### Repository Structure
- `bootstrap/`: ArgoCD Application definitions
- `apps/`: Application manifests and configurations
  - `apps/argocd/`: Helm chart for ArgoCD itself  
  - `apps/prometheus/`: Raw Kubernetes manifests for Prometheus monitoring
  - `apps/grafana/`: Raw Kubernetes manifests for Grafana dashboards
  - `apps/rancher/`: Helm values for Rancher management platform
  - Other apps: linkding, homepage, uptime-kuma, local-path-provisioner

### Application Types
- **Helm Charts**: Used for ArgoCD and Rancher (with Chart.yaml and values.yaml)
- **Raw Manifests**: Used for most apps like Prometheus, Grafana (direct YAML files)
- **Kustomize**: Used for local-path-provisioner (with kustomization.yaml)

## Domain Configuration
Applications are configured to use the `elam.sh` domain with nginx ingress controller:
- prometheus.elam.sh
- grafana.elam.sh  
- rancher.elam.sh

## Common Patterns
- All applications use automated sync with prune and self-heal enabled
- Persistent storage uses `local-path` storage class
- Services expose standard ports with ingress for external access
- Default credentials are often set to admin/admin for homelab convenience