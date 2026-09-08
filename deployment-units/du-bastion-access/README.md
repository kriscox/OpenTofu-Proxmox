# Bastion Access

## Purpose

Provide controlled administrative access from the management network to protected platform networks.

## Scope

### In scope

- Bastion/jump-host VM.
- Administrative SSH access and ProxyJump capability.
- Required host bootstrap and monitoring integration.

### Out of scope

- Kubernetes cluster ownership.
- General-purpose tooling or collaboration services.
- Application access paths.

## Ownership and lifecycle

- **Owner:** IT Operations / Platform Infrastructure
- **Lifecycle:** Administrative-access lifecycle independent from workload platforms.

## Boundaries

- Isolated VM with explicitly controlled interfaces between management and protected platform networks.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Administrative SSH | Inbound | Receive controlled operator access from the management network. |
| Protected-platform access | Outbound | Reach explicitly permitted management endpoints in consumer scopes. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| `du-proxmox-networking` | Provides required network segments. |
| `du-proxmox-firewall` | Enforces permitted administrative connectivity. |

## Constraints

- The bastion must not become a general-purpose tools host.
- Access must follow least-privilege network policy.

## Decisions and deviations

***No deployment-unit-specific decisions or deviations.***
