# Kubernetes Workers

## Purpose

Provide independently scalable Kubernetes worker capacity for application and platform workloads.

## Scope

### In scope

- Worker capacity required by the selected environment implementation.
- Kubernetes worker installation or managed node-pool configuration.
- Worker registration with the cluster where applicable.
- Worker-specific bootstrap and health verification.

### Out of scope

- Kubernetes control-plane initialization and lifecycle.
- Application workload deployment.
- Environment-specific infrastructure foundation implementation and enforcement.

## Ownership and lifecycle

- **Owner:** Platform Operations
- **Lifecycle:** Worker capacity may be scaled, replaced or changed independently from the control plane where the selected platform implementation exposes that lifecycle.

## Boundaries

- Bounded to worker capacity registered with one Kubernetes cluster, regardless of whether workers are self-managed virtual machines or provider-managed node pools.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Kubernetes control plane | Outbound | Register workers and receive cluster scheduling/configuration. |
| Provisioned node contract | Inbound | Receive generic node identity and addressing information when the implementation uses self-managed worker nodes. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| `du-kubernetes-control-plane` | Provides an initialized Kubernetes cluster to join or attach worker capacity to. |
| Environment infrastructure capabilities | Provide the compute, networking, identity and security prerequisites required by the selected implementation. |

## Constraints

- Worker capacity must be independently scalable and replaceable where supported by the selected implementation.
- Kubernetes implementation selection is environment-specific and must not define the deployment-unit boundary.

## Decisions and deviations

- DEV currently uses self-managed RKE2 workers on Proxmox; managed Kubernetes services such as AKS may provide the same capability through managed node pools in other environments.
