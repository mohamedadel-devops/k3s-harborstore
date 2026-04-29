# 🚀 HarborStore -- Production-Ready Microservices on k3s

HarborStore is a cloud-native microservices web store deployed on a
self-managed **k3s Kubernetes cluster on AWS EC2**.

This project demonstrates:

-   Production-style CI/CD
-   Manual, versioned production releases
-   Hardened container security
-   Horizontal autoscaling
-   Kubernetes-native architecture
-   Ingress-based routing
-   ConfigMap-based configuration
-   Secrets management best practices

------------------------------------------------------------------------

# 🏗️ Architecture

Internet\
↓\
NGINX Ingress Controller\
↓\
Gateway (NGINX reverse proxy)\
↓\
Catalog Service \| Cart Service\
↓ ↓\
Redis PostgreSQL

------------------------------------------------------------------------

# ☁️ Infrastructure

-   Kubernetes: **k3s**
-   Hosting: **AWS EC2**
-   Networking: NodePort + Ingress
-   Storage: local-path PVC
-   Namespace isolation: `harborstore`

------------------------------------------------------------------------

# 🔐 Security Hardening

All workloads include:

-   runAsNonRoot
-   Dropped Linux capabilities
-   readOnlyRootFilesystem
-   Explicit CPU/memory requests & limits
-   ConfigMaps for non-sensitive configuration
-   Secrets stored server-side (not in GitHub)

------------------------------------------------------------------------

# ⚙️ CI/CD Pipeline

## CI -- Validate Kubernetes Manifests

-   Runs on Pull Requests
-   Validates YAML syntax using Python
-   Prevents broken manifests from merging

## CD -- Manual Production Deploy (Tag-Based)

Production deployment:

-   Manual (`workflow_dispatch`)
-   Versioned (must deploy a `vX.Y.Z` tag)
-   Requires GitHub Environment approval
-   SSH-based deployment to k3s control plane
-   Deploys exact tagged commit
-   Generates deployment summary
-   Concurrency protected

### Release Process

``` bash
git checkout main
git pull
git tag -a v1.0.0 -m "HarborStore v1.0.0"
git push origin v1.0.0
```

Then trigger deploy in GitHub Actions using the tag.

------------------------------------------------------------------------

# 📈 Autoscaling

Horizontal Pod Autoscaler (HPA) configured for:

-   Gateway service
-   CPU-based scaling
-   Target: 60% CPU utilization
-   Scale range: 1 → 5 replicas
-   Stabilized scale-down behavior

------------------------------------------------------------------------

# 🔎 Health & Observability

All services include:

-   startupProbe
-   readinessProbe
-   livenessProbe
-   Structured access logging
-   Request tracing headers

------------------------------------------------------------------------

# 📂 Repository Structure

    k8s/
     ├── 00-namespace/
     ├── 01-configmaps/
     ├── 02-secrets/
     ├── 03-database/
     ├── 04-redis/
     ├── 05-services/
     ├── 06-ingress/
     └── 07-hpa/
    scripts/
     ├── deploy.sh
     └── destroy.sh
    .github/
     └── workflows/
          ├── ci.yaml
          └── cd.yaml

------------------------------------------------------------------------

# 🧠 What This Project Demonstrates

This project showcases:

-   Declarative Kubernetes management
-   Secure workload configuration
-   Manual production control gates
-   Version-pinned deployments
-   Cloud-native scaling behavior
-   Real-world debugging & rollout practices

------------------------------------------------------------------------

# 👨‍💻 Author

Mohamed Adel Asar\
DevOps Engineer (Aspiring)\
AWS \| Kubernetes \| CI/CD \| Linux
