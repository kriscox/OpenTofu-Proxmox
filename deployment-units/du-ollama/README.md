# Ollama

## Purpose

Provide an isolated Ollama test capability with an independent deployment lifecycle.

## Scope

### In scope

- Ollama VM infrastructure.
- Minimal operating-system bootstrap.
- Ollama installation and service startup.
- Minimal service health verification.

### Out of scope

- Kubernetes infrastructure.
- Production-grade AI platform capabilities.
- Consumer workload configuration.

## Ownership and lifecycle

- **Owner:** Platform Operations
- **Lifecycle:** Test capability that can be deployed, changed or removed independently.

## Boundaries

- Bounded to a dedicated VM and Ollama service endpoint.

## Interfaces

| Interface | Direction | Purpose |
| --- | --- | --- |
| Ollama API | Inbound | Allow approved consumers to use the inference service. |

## Dependencies

| Dependency | Reason |
| --- | --- |
| Proxmox foundation capabilities | Provides networking, firewall enforcement and VM bootstrap prerequisites. |

## Constraints

- The implementation should remain deliberately lightweight while this is a test capability.

## Decisions and deviations

- A simple deployment script is acceptable as the delivery mechanism for the current test use case.
