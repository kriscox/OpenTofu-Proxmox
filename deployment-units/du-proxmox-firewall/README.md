# Proxmox Firewall

## Purpose

Provide host-level firewall, forwarding and NAT enforcement for Proxmox-hosted platform networks.

## Scope

### In scope

- nftables or equivalent host firewall enforcement.
- Default-deny forwarding baseline.
- NAT and masquerading where required.
- Enforcement of firewall requirements supplied by consumer scopes.

### Out of scope

- Ownership of consumer-specific firewall requirements.
- Kubernetes network policies.
- Application-level authorization.

## Ownership and lifecycle

- **Owner:** IT Operations / Platform Infrastructure
- **Lifecycle:** Host security lifecycle; policy changes may be applied independently while respecting dependent-scope requirements.

## Boundaries

- Bounded to firewall and forwarding enforcement on the Proxmox host.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Firewall requirements | Inbound | Receive declarative connectivity requirements from consumer scopes. |
| Enforced network policy | Outbound | Apply approved connectivity on managed platform networks. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| `du-proxmox-networking` | Provides the network segments and routing on which policy is enforced. |

## Constraints

- Consumer scopes retain ownership of their connectivity requirements.
- The firewall implementation must not embed workload-specific logic beyond supplied policy data.

## Decisions and deviations

- Policy enforcement is owned here; policy intent remains with the consumer scope.
