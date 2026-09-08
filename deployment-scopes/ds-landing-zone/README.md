# Landing Zone

## Purpose

Provide controlled administrative access capabilities required to operate protected platform networks and higher deployment scopes.

## Scope

### In scope

- Bastion/jump-host infrastructure.
- Controlled administrative access from the management network to protected platform networks.
- SSH ProxyJump capability and equivalent administrative access patterns where required.
- Integration with central infrastructure monitoring where required.

### Out of scope

- Kubernetes infrastructure and cluster configuration.
- Ollama infrastructure or service configuration.
- General application or collaboration tools.
- Central monitoring infrastructure itself.

## Composition

| Deployment Unit | Role |
| --- | --- |
| `du-bastion-access` | Provides controlled administrative access to protected platform networks. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| `ds-proxmox-foundation` | Provides networking, firewall enforcement and VM bootstrap prerequisites. |

## Constraints

- The landing zone must remain limited to generic administrative access capabilities.
- Workload-specific tooling must not be added solely because it is shared.
- Required firewall policy remains owned by this consumer scope and is enforced by the foundation.

## Decisions and deviations

***No scope-specific decisions or deviations.***
