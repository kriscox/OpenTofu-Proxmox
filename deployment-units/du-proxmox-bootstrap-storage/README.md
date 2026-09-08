# Proxmox Bootstrap Storage

## Purpose

Provide the minimal Proxmox snippet-storage capability required to bootstrap virtual machines.

## Scope

### In scope

- Enabling and exposing Proxmox snippet storage required for VM initialization data.

### Out of scope

- General-purpose workload storage.
- Kubernetes persistent storage.
- Backup and retention platforms.

## Ownership and lifecycle

- **Owner:** IT Operations / Platform Infrastructure
- **Lifecycle:** Proxmox foundation lifecycle; expected to change infrequently.

## Boundaries

- Bounded to the Proxmox storage capability used for VM bootstrap snippets.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Snippet storage | Outbound | Allow VM provisioning mechanisms to provide initialization data. |

## Dependencies

No deployment-unit dependencies beyond the underlying Proxmox storage platform.

## Constraints

- This unit must remain limited to bootstrap storage; broader storage responsibilities require separate architectural evaluation.

## Decisions and deviations

***No deployment-unit-specific decisions or deviations.***
