# Ollama

## Purpose

Provide an isolated Ollama test deployment that can be deployed and removed independently from the Kubernetes platform and other platform scopes.

## Scope

### In scope

- Ollama VM infrastructure.
- Minimal OS/bootstrap configuration required by Ollama.
- Ollama installation and service startup.
- Minimal health verification.
- Integration with central infrastructure monitoring where useful.

### Out of scope

- Kubernetes infrastructure or workloads.
- Proxmox host networking and firewall enforcement.
- Shared platform tooling.
- Production-grade AI platform capabilities beyond the test deployment.

## Composition

| Deployment Unit | Role |
| --- | --- |
| `du-ollama` | Provides the complete isolated Ollama test capability. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| `ds-proxmox-foundation` | Provides network, firewall enforcement and VM bootstrap prerequisites. |

## Constraints

- Ollama is currently a test capability and should remain deliberately lightweight.
- It must not be coupled to Kubernetes deployment or lifecycle.
- Consumer scopes own any firewall requirements for consuming the Ollama API.

## Decisions and deviations

- The deployment is intentionally kept as one deployment unit and may use a simple deployment script rather than a more complex delivery model.
