# Kubernetes Control Plane

## Purpose

Provide the Kubernetes control-plane capability and initialize the Kubernetes cluster.

## Scope

### In scope

- Control-plane capability required by the selected environment implementation.
- Kubernetes control-plane installation or managed-service configuration.
- Cluster initialization and additional control-plane registration where applicable.
- API endpoint and kubeconfig generation/configuration.
- Cluster-wide base configuration required to establish a healthy cluster.
- Base cluster health verification.

### Out of scope

- Independently scalable worker capacity.
- Application workloads.
- Environment-specific infrastructure foundation implementation and enforcement.

## Ownership and lifecycle

- **Owner:** Platform Operations
- **Lifecycle:** Kubernetes control-plane and cluster lifecycle; upgrades may occur independently from worker scaling where the selected platform implementation exposes that lifecycle.

## Boundaries

- Bounded to Kubernetes control-plane responsibilities for one cluster, regardless of whether the control plane is self-managed or provided as a managed service.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Kubernetes API | Inbound | Provide cluster management interface. |
| Worker registration | Inbound | Allow workers to join the initialized cluster. |
| Provisioned node contract | Inbound | Receive generic node identity and addressing information when the implementation uses self-managed control-plane nodes. |

## Bootstrap sequence

For self-managed implementations, the control-plane bootstrap sequence is owned by this deployment unit and is implemented by its bootstrap scripts.

The sequence is:

1. consume the generic provisioned node contract containing at minimum hostname, role and IP address;
2. bootstrap the first control-plane node and initialize the cluster;
3. obtain or generate the cluster registration information required by additional control-plane nodes;
4. join the remaining control-plane nodes;
5. expose the Kubernetes API endpoint and kubeconfig;
6. verify that the control plane is healthy before worker registration is allowed.

For managed Kubernetes services, the provider-specific control-plane lifecycle replaces the self-managed bootstrap sequence while preserving the same deployment-unit responsibility and external interfaces.

The provisioning layer must not produce Ansible-specific inventory or Kubernetes-distribution-specific join data as part of its architectural contract. Distribution- and provider-specific bootstrap details remain internal to this deployment unit and its implementation.

## Dependencies

| Dependency | Reason |
| --- | --- |
| Environment infrastructure capabilities | Provide the compute, networking, identity and security prerequisites required by the selected implementation. |
| Controlled administrative access | Provides operational access where required; this is not a universal hard architectural dependency. |

## Constraints

- Kubernetes implementation selection is environment-specific and must not define the deployment-unit boundary.
- Control-plane availability requirements determine node count and topology where self-managed.
- Bootstrap scripts must remain purpose-specific and must not evolve into a general configuration-management layer.

## Decisions and deviations

- Cluster initialization is included in this unit rather than modelled as a separate deployment unit.
- Control-plane bootstrap orchestration is part of this deployment unit and is not modelled as a separate platform dependency.
- DEV currently uses a self-managed RKE2 implementation on Proxmox; managed Kubernetes services such as AKS may implement the same capability in other environments.
