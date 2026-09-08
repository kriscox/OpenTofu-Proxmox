# Kubernetes Control Plane

## Purpose

Provide the Kubernetes control-plane infrastructure and initialize the Kubernetes cluster.

## Scope

### In scope

- Control-plane virtual machines and their compute/storage/network configuration.
- Kubernetes control-plane installation.
- Cluster initialization and additional control-plane registration.
- API endpoint and kubeconfig generation/configuration.
- Cluster-wide base configuration required to establish a healthy cluster.
- Base cluster health verification.

### Out of scope

- Independently scalable worker capacity.
- Application workloads.
- Proxmox host networking and firewall enforcement.

## Ownership and lifecycle

- **Owner:** Platform Operations
- **Lifecycle:** Kubernetes control-plane and cluster lifecycle; upgrades may occur independently from worker scaling.

## Boundaries

- Bounded to control-plane compute and Kubernetes control-plane responsibilities for one cluster.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Kubernetes API | Inbound | Provide cluster management interface. |
| Worker registration | Inbound | Allow workers to join the initialized cluster. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| Proxmox foundation capabilities | Provides compute prerequisites, networking and firewall enforcement. |
| Bastion access capability | Provides controlled administrative access where required. |

## Constraints

- Kubernetes distribution selection must remain an implementation choice based on platform requirements.
- Control-plane availability requirements determine node count and topology.

## Decisions and deviations

- Cluster initialization is included in this unit rather than modelled as a separate deployment unit.
