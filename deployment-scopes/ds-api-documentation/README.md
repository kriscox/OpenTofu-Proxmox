# API Documentation

## Purpose

Compose and document the deployable boundary for publishing the API documentation UI on Kubernetes (local K3s first, Proxmox-hosted environments later).

## Scope

### In scope

- Composition of `du-api-documentation` as a single application scope
- Declaration of dependencies on cluster runtime, Ingress, and contracts storage
- Pointer to Helm-based deployment in the API-documentation repository

### Out of scope

- OpenTofu resources for VMs, storage, or Helm releases (deferred)
- Shared contracts platform implementation
- Cluster bootstrap and Ingress-controller installation

## Composition

| Deployment Unit | Role |
| --------------- | ---- |
| `du-api-documentation` | ReDoc documentation UI, optional WAF, Ingress exposure |

## Dependencies

| Dependency | Reason |
| ---------- | ------ |
| Kubernetes runtime (cluster / K3s) | Executes the Helm-released workloads |
| Ingress controller | Provides external HTTP routing (Traefik on K3s) |
| Shared contracts storage | Supplies read-only OpenAPI content (location TBD) |

## Constraints

- Application Helm chart and Docker artefacts remain in `API-documentation/infra/`; this scope does not duplicate them
- Contracts storage location is not fixed yet; the Helm chart mounts a PVC or hostPath until the shared platform path is chosen
- No OpenTofu state is managed for this scope in the current phase

## Decisions and deviations

- OpenTofu implementation is deferred; Helm in the application repository is the active deploy path
- Local validation target is K3s with Traefik Ingress before Proxmox shared environments
