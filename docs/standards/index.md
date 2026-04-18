# Architecture & Standards

This section contains our standard operating procedures, architectural conventions, and naming standards to ensure consistency across the platform.

## Architecture

Below is a sample architecture representing how incoming traffic routes to our microservices via our standard API Gateway and Kubernetes Ingress.

```mermaid
graph TD
    Client(("Client")) --> Cloudflare["Cloudflare CDN / WAF"]
    Cloudflare --> ALB["AWS Application Load Balancer"]
    
    subgraph k8s_cluster ["Kubernetes Cluster"]
        NginxIngress["Ingress Controller (NGINX)"]
        Svc1["Frontend Service"]
        Svc2["API Gateway Service"]
        Pod1("Frontend Pods")
        Pod2("Auth Service")
        Pod3("Billing Service")
        
        NginxIngress --> Svc1
        NginxIngress --> Svc2
        Svc1 --> Pod1
        Svc2 --> Pod2
        Svc2 --> Pod3
    end
    
    ALB --> NginxIngress
    
    Pod2 --> Redis[("Redis Cache")]
    Pod3 --> DB[("PostgreSQL")]

    classDef aws_style fill:#FF9900,stroke:#232F3E,stroke-width:2px,color:white;
    classDef k8s_style fill:#326CE5,stroke:#fff,stroke-width:2px,color:white;
    classDef db_style fill:#336791,stroke:#fff,stroke-width:2px,color:white;
    
    class ALB aws_style;
    class NginxIngress,Svc1,Svc2 k8s_style;
    class DB db_style;
```

## Naming Conventions

Consistency makes automation easier. Please adhere to the following when creating new resources:

### AWS Resources

Use the format: `<environment>-<region>-<service>-<name>`

*   **Example (S3):** `prod-euwest1-s3-applogs`
*   **Example (ALB):** `staging-useast1-alb-api`

### Kubernetes Namespaces

Namespaces should reflect the team or the primary generic function.

*   `monitoring` (Prometheus, Grafana)
*   `ingress-nginx` (Ingress controllers)
*   `team-billing`
*   `team-auth`

## GitOps Workflow

We practice GitOps. Direct cluster mutation via `kubectl apply` is disabled in `staging` and `prod`.

1.  Make a branch modifying the `k8s-manifests` repo.
2.  Open a Pull Request.
3.  CI runs `kubeval` and `conftest` policies.
4.  Once approved and merged to `main`, ArgoCD will automatically sync the changes.

## Terraform Best Practices

*   Never use local state. Always use the configured S3 backend with DynamoDB locking.
*   Pin module versions in the `main.tf` files.
*   Always run `terraform plan` and attach the output to the PR for review.
