# Proxmox Networking

## Purpose

Provide the Proxmox host networking foundation used by platform deployment scopes.

## Scope

### In scope

- VLAN-aware bridge configuration.
- Proxmox host VLAN interfaces and gateways.
- Routing and IP forwarding required between managed network segments.

### Out of scope

- Firewall policy enforcement.
- Consumer-specific connectivity requirements.
- Workload networking inside Kubernetes.

## Ownership and lifecycle

- **Owner:** IT Operations / Platform Infrastructure
- **Lifecycle:** Host-level infrastructure lifecycle; changes may affect all dependent scopes.

## Boundaries

- Bounded to Proxmox host networking and layer-3 routing capabilities managed on the hypervisor.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Network segments and gateways | Outbound | Provide routable platform network segments to consumer scopes. |
| Upstream network | Outbound | Provide management and external connectivity. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| Upstream network | Provides physical/external network connectivity. |

## Constraints

- Management connectivity must be protected during network changes.
- Configuration must support controlled rollback.

## Decisions and deviations

***No deployment-unit-specific decisions or deviations.***
