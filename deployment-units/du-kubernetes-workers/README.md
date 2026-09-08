# Kubernetes Workers

## Purpose

Provide independently scalable Kubernetes worker capacity for application and platform workloads.

## Scope

### In scope

- Worker virtual machines and their compute/storage/network configuration.
- Kubernetes worker installation and registration with the cluster.
- Worker-specific bootstrap and health verification.

### Out of scope

- Kubernetes control-plane initialization and lifecycle.
- Application workload deployment.
- Proxmox host networking and firewall enforcement.

## Ownership and lifecycle

- **Owner:** Platform Operations
- **Lifecycle:** Worker capacity may be scaled, replaced or changed independently from the control plane.

## Boundaries

- Bounded to worker compute capacity registered with one Kubernetes cluster.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Kubernetes control plane | Outbound | Register workers and receive cluster scheduling/configuration. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| `du-kubernetes-control-plane` | Provides an initialized Kubernetes cluster to join. |
| Proxmox foundation capabilities | Provides compute prerequisites, networking and firewall enforcement. |

## Constraints

- Worker count and sizing must be changeable without requiring control-plane replacement.

## Decisions and deviations

***No deployment-unit-specific decisions or deviations.***
