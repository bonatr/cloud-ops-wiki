# Getting Started

Welcome to the Cloud Operations & Platform Engineering team! This guide will help you set up your local environment and get productive right away.

## Day 1 Checklist

- [ ] Request Identity and Access Management (IAM) Roles via [IT Portal](#).
- [ ] Connect to the corporate VPN.
- [ ] Join the `#platform-eng-alerts` and `#cloud-ops-general` Slack channels.
- [ ] Install your preferred IDE or Cloud Editor.

## Development Environment Setup

Ensure you have the following tools installed locally to interact with our infrastructure:

=== "macOS"

    ```bash
    brew install kubectl
    brew install k9s
    brew install terraform
    brew install awscli
    ```

=== "Linux"

    ```bash
    # Install curl, unzip, and other dependencies before proceeding
    sudo apt-get update && sudo apt-get install -y curl unzip
    # Follow official guides to install kubectl, terraform, awscli...
    ```

=== "Windows"

    ```powershell
    winget install Kubernetes.kubectl
    winget install Hashicorp.Terraform
    winget install Amazon.AWSCLI
    ```

## Local Sandbox

We use **k3d** for local Kubernetes testing. Run the following to spin up a quick development cluster:

```bash
k3d cluster create ops-sandbox --servers 1 --agents 2
```

> [!TIP]
> Make sure Docker is running on your machine before attempting to spin up the local cluster.

## Core Repositories

| Repository | Purpose | Ownership |
| ---------- | ------- | --------- |
| `cloud-ops-infra` | Main Terraform configuration for AWS / GCP | Platform Team |
| `k8s-manifests` | Flux/ArgoCD GitOps cluster state | Platform Team |
| `internal-tools-cli` | Go-based utility wrappers for internal APIs | Developer Experience |
