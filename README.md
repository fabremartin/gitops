# GitOps Repository

This repository contains the Kubernetes manifests used for GitOps-based deployments in our infrastructure.

## Overview

I use a GitOps approach to manage our Kubernetes resources. This repository is the single source of truth for our infrastructure configuration. Changes to manifests in this repository trigger automatic deployments to the Kubernetes clusters via FluxCD.

## Repository Structure

```
Gitops Repo
├── helm-repositories         # Helm repository configuration and secrets
│   ├── repository.yaml       # Definition of the Helm repository
│
├── services/json-server      # Manifests for json-server
│   ├── kustomization.yaml    # Kustomization file for json-server
│   ├── HelmRelease.yaml      # HelmRelease for json-server
│   ├── values.yaml           # Custom Helm values for json-server
│
├── kustomization.yaml    # Kustomization file for FluxCD


```

## Global architecture schema

![image](https://github.com/user-attachments/assets/415bcd12-1d03-4ce8-ad9e-86839cd98818)


## Workflow

The key workflow for this repository is:

1. Changes to application code in microservice/app repositories trigger their CI pipelines
2. Those CI pipelines build and tag docker images then pushes it to the ACR
3. The CI pipeline then updates the corresponding manifest in this GitOps repository with the new image tag and chart version
4. The GitOps operator (Flux) detects changes and applies them to the cluster

This approach ensures that:
- Application repositories are responsible for their own deployment updates
- All infrastructure changes are versioned and tracked in this central repository
- Deployments happen automatically when manifests are updated


## Prerequisites

- kubectl
- git
- Azure CLI (for accessing Azure resources)

## Links
- [iac repo](https://github.com/fabremartin/iac)
- [gitops repo](https://github.com/fabremartin/gitops)
- [json-server repo](https://github.com/fabremartin/json-server)
