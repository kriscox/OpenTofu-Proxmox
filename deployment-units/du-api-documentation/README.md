# API Documentation

## Purpose

Publish a browsable ReDoc UI for Parking.brussels API contracts so partners and internal teams can discover and inspect versioned OpenAPI specifications.

## Scope

### In scope

- Nginx-hosted ReDoc documentation UI
- Helm packaging and Kubernetes deployment artefacts (maintained in the API-documentation application repository under `infra/`)
- Optional OWASP ModSecurity WAF in front of the documentation backend
- HTTP(S) exposure via Kubernetes Ingress

### Out of scope

- Provisioning of the Kubernetes cluster or Ingress controller
- Provisioning of shared contracts storage (location TBD on the platform)
- Contract authoring workflow and partner onboarding processes
- OpenTofu implementation for this unit (deferred to a later platform phase)

## Ownership and lifecycle

- **Owner:** Kris Cox / API documentation maintainers
- **Lifecycle:** Application release cycle via the API-documentation repository (image + Helm chart); platform wiring follows cluster and shared-storage availability

## Boundaries

- Application source, Docker image, Compose, and Helm chart live in the API-documentation repository (`infra/`)
- Runtime contracts content is supplied externally at `/opt/contracts` and is not packaged in the container image
- Network exposure is limited to the Ingress entrypoint (and optional WAF) within the target cluster

## Interfaces

| Interface                                     | Direction | Purpose                                             |
| --------------------------------------------- | --------- | --------------------------------------------------- |
| HTTP(S) documentation UI                      | Inbound   | Serve ReDoc UI and OpenAPI files to clients         |
| Contracts filesystem mount (`/opt/contracts`) | Inbound   | Read-only OpenAPI tree from shared or local storage |
| Container image / Helm chart                  | Outbound  | Deployable artefacts for Kubernetes                 |

## Dependencies

| Dependency                               | Reason                                           |
| ---------------------------------------- | ------------------------------------------------ |
| Kubernetes runtime                       | Hosts the documentation workload                 |
| Ingress controller (e.g. Traefik on K3s) | Routes external HTTP to the service              |
| Shared contracts storage                 | Supplies OpenAPI files at runtime (location TBD) |
| Container registry                       | Stores the documentation image                   |

## Constraints

- Contracts must not be baked into the runtime image
- Deploy and infra artefacts for this application must remain under API-documentation `infra/`
- Ingress host, TLS, and PVC claim are environment-specific overlays (`values-k3s.yaml`, `values-proxmox.yaml`)

## Decisions and deviations

- Helm (not OpenTofu) is the packaging and deploy mechanism for this application until platform OpenTofu scopes manage cluster-side resources
- Classic `networking.k8s.io/v1` Ingress is used rather than Gateway API HTTPRoute for K3s Traefik compatibility
