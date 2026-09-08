# Proxmox Foundation

## Purpose

Provide the host-level Proxmox infrastructure foundation required by higher deployment scopes, including network segmentation, routing, firewall enforcement and VM bootstrap storage prerequisites.

## Scope

### In scope

- Proxmox host networking and VLAN configuration.
- VLAN gateways, routing and IP forwarding.
- Host firewall and NAT enforcement.
- Proxmox snippet storage required for VM bootstrap.
- Integration with central infrastructure monitoring where required.

### Out of scope

- Bastion and administrative access hosts.
- Kubernetes compute and cluster configuration.
- Ollama infrastructure or service configuration.
- Central monitoring infrastructure itself.
- Ownership of consumer-specific firewall requirements.

## Composition

| Deployment Unit | Role |
| --- | --- |
| `du-proxmox-networking` | Provides host networking, VLAN gateways and routing. |
| `du-proxmox-firewall` | Enforces host firewall, forwarding and NAT policies. |
| `du-proxmox-bootstrap-storage` | Provides Proxmox snippet storage required for VM bootstrap. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| External/upstream network | Provides host management and upstream connectivity. |
| Central infrastructure monitoring | Consumed where monitoring integration is required. |

## Constraints

- The foundation must be deployable without knowledge of Kubernetes, Ollama or other higher-level workloads.
- Firewall requirements are owned by the consumer scope and supplied to the foundation for enforcement.
- Changes to host networking and firewall configuration can affect all dependent scopes and require controlled validation and rollback.

## Decisions and deviations

- Firewall capability ownership belongs to this scope; consumer-specific firewall requirements remain with the consumer scope.
- Central monitoring is an external shared capability rather than a deployment unit within this scope.
