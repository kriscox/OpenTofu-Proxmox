# Kubernetes Platform

## Purpose

Provide a Kubernetes platform with independently manageable control-plane and worker capacity.

## Scope

### In scope

- Kubernetes control-plane capability and cluster initialization.
- Kubernetes worker capacity and worker registration.
- Kubernetes distribution or managed-service configuration required to provide the platform.
- Cluster-wide base configuration.
- Cluster-specific networking and bootstrap requirements.
- Integration with central infrastructure monitoring where required.
- Firewall and connectivity requirements required by the Kubernetes platform.

### Out of scope

- Environment-specific infrastructure foundation implementation.
- Bastion infrastructure.
- Application workloads deployed onto Kubernetes.
- Ollama infrastructure or service configuration.

## Composition

| Deployment Unit | Role |
| --- | --- |
| `du-kubernetes-control-plane` | Provides the Kubernetes control-plane capability and cluster initialization. |
| `du-kubernetes-workers` | Provides independently scalable worker capacity and worker registration. |

## Dependencies

The scope has no infrastructure-provider-specific hard dependency that applies to every environment.

Environment implementations declare their own infrastructure prerequisites. For example:

- DEV may consume Proxmox foundation capabilities and use RKE2;
- ACC and PRD may consume Azure infrastructure capabilities and use a managed Kubernetes service such as AKS.

Controlled administrative access may be provided through the landing-zone capability where required, but this is an operational relationship rather than a hard architectural dependency of the Kubernetes platform.

## Constraints

- Worker capacity must be independently scalable and replaceable from the control plane where the selected platform implementation exposes that lifecycle.
- Kubernetes implementation choice is environment-specific and must not define the deployment-scope boundary.
- Infrastructure-specific firewall and connectivity requirements are owned by this scope and supplied to the relevant environment foundation for enforcement.
- Provider-specific implementation details must remain outside the generic scope contract.

## Decisions and deviations

- Cluster initialization and cluster-wide base configuration are part of the control-plane deployment unit rather than a separate cluster deployment unit.
- DEV currently targets Proxmox with RKE2; this does not constrain ACC or PRD to the same implementation.
