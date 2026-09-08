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
| Provisioned node contract | Inbound | Receive generic node identity and addressing information from VM provisioning. |

## Bootstrap sequence

The control-plane bootstrap sequence is owned by this deployment unit and is implemented by its bootstrap scripts.

The sequence is:

1. consume the generic provisioned node contract containing at minimum hostname, role and IP address;
2. bootstrap the first control-plane node and initialize the cluster;
3. obtain or generate the cluster registration information required by additional control-plane nodes;
4. join the remaining control-plane nodes;
5. expose the Kubernetes API endpoint and kubeconfig;
6. verify that the control plane is healthy before worker registration is allowed.

The provisioning layer must not produce Ansible-specific inventory or Kubernetes-distribution-specific join data as part of its architectural contract. Distribution-specific bootstrap details remain internal to this deployment unit and its scripts.

## Dependencies

| Dependency | Reason |
| --- | --- |
| Proxmox foundation capabilities | Provides compute prerequisites, networking and firewall enforcement. |
| Bastion access capability | Provides controlled administrative access where required. |

## Constraints

- Kubernetes distribution selection must remain an implementation choice based on platform requirements.
- Control-plane availability requirements determine node count and topology.
- Bootstrap scripts must remain purpose-specific and must not evolve into a general configuration-management layer.

## Decisions and deviations

- Cluster initialization is included in this unit rather than modelled as a separate deployment unit.
- Control-plane bootstrap orchestration is part of this deployment unit and is not modelled as a separate platform dependency.
