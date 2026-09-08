# Kubernetes Platform

## Purpose

Provide a Kubernetes platform on Proxmox with independently manageable control-plane and worker capacity.

## Scope

### In scope

- Kubernetes control-plane compute and cluster initialization.
- Kubernetes worker compute and worker registration.
- Kubernetes distribution installation and cluster-wide base configuration.
- Cluster-specific networking and bootstrap requirements.
- Integration with central infrastructure monitoring where required.
- Firewall requirements required by the Kubernetes platform.

### Out of scope

- Proxmox host networking and firewall enforcement.
- Bastion infrastructure.
- Application workloads deployed onto Kubernetes.
- Ollama infrastructure or service configuration.

## Composition

| Deployment Unit | Role |
| --- | --- |
| `du-kubernetes-control-plane` | Provides control-plane compute, Kubernetes control plane and cluster initialization. |
| `du-kubernetes-workers` | Provides independently scalable worker capacity and worker registration. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| `ds-proxmox-foundation` | Provides network, firewall enforcement and VM bootstrap prerequisites. |
| `ds-landing-zone` | Provides controlled administrative access where required for platform operations. |

## Constraints

- Worker capacity must be independently scalable and replaceable from the control plane.
- Kubernetes distribution choice must follow platform requirements and must not define the deployment-scope boundary.
- Firewall requirements are owned by this scope and supplied to the foundation for enforcement.

## Decisions and deviations

- Cluster initialization and cluster-wide base configuration are part of the control-plane deployment unit rather than a separate cluster deployment unit.
