# ADR-005: Define OpenTofu Module Principles

* Status: Proposed
* Date: 2026-08-18
* Owners: Technical Architect

## Context

Deployment units define architectural building blocks, while deployment scopes compose those units into OpenTofu management boundaries.

OpenTofu modules serve a different purpose. They are technical implementation building blocks used to encapsulate and reuse infrastructure capabilities.

The module strategy must therefore avoid creating an artificial one-to-one relationship between architecture boundaries and OpenTofu implementation constructs.

It must also support portability, replaceability and controlled evolution of shared technical capabilities.

## Decision

OpenTofu modules represent **coherent, reusable technical capabilities**.

Modules are not defined by deployment-unit or deployment-scope boundaries. A deployment unit or scope may use multiple modules, and the same module may be reused by multiple deployment units or scopes.

A module should only be introduced when it provides a meaningful technical abstraction, reuse value or useful encapsulation. Resources do not need to be wrapped in modules solely for structural consistency.

### Provider-specific implementation

Provider- or platform-specific implementation details should be encapsulated behind module boundaries where this improves portability, replaceability or maintainability.

Modules may intentionally remain provider-specific when attempting to create a generic cross-provider abstraction would introduce unnecessary complexity or hide useful provider capabilities.

### Module interface

Modules must expose explicit inputs and outputs as their interface.

Consumers of a module must not depend on its internal resource structure or implementation details.

Internal module changes should therefore remain possible without requiring changes to consuming deployment scopes as long as the module interface remains compatible.

### Module versioning

Modules must be independently versionable.

Deployment scopes should be able to reference an explicit module version rather than implicitly depending on the latest module implementation.

This allows module upgrades to be introduced and validated independently across deployment scopes.

The mechanism used to implement module versioning is intentionally not defined by this decision.

## Alternatives considered

### One module per deployment unit

Each deployment unit could be implemented as one OpenTofu module.

This was rejected because deployment units define architectural responsibility and lifecycle boundaries, while modules define reusable technical implementation capabilities.

Forcing these boundaries to match would reduce reuse and create unnecessary coupling between architecture and implementation.

### Wrap all provider-specific resources in modules

Every provider-specific resource could be required to exist inside a module.

This was rejected because it would create unnecessary wrapper modules for simple or scope-specific resources and would conflict with the principle of simplicity over complexity.

Provider-specific implementation should be encapsulated when doing so provides meaningful architectural or technical value.

### Generic cross-provider modules

Modules could attempt to expose one generic abstraction across multiple infrastructure providers.

This was rejected as a default approach because provider capabilities and semantics often differ substantially.

Cross-provider abstractions may be introduced when a genuinely stable and useful common capability exists, but they are not required by the module strategy.

## Consequences

### Positive

* Technical capabilities can be reused across deployment units and scopes.
* Architectural boundaries remain independent from OpenTofu implementation boundaries.
* Provider-specific implementation details can be isolated where useful.
* Consumers remain decoupled from module internals.
* Module implementations can evolve while preserving stable interfaces.
* Deployment scopes can adopt module upgrades independently.

### Negative

* Determining when a technical capability justifies a module requires judgement.
* Provider-specific modules may result in multiple implementations of conceptually similar capabilities.
* Independent module versioning introduces lifecycle and dependency-management requirements.

## Risks

* Excessive module creation may create unnecessary abstraction and maintenance overhead.
* Modules may become overly generic in an attempt to support multiple providers.
* Internal module details may leak into deployment scopes if module interfaces are poorly designed.
* Breaking interface changes may create significant upgrade effort across consuming deployment scopes.

## Follow-up actions

* [X] Define the repository structure independently from this decision.
* [ ] Define the module versioning mechanism.
* [ ] Define module coding and interface conventions.
* [ ] Validate the module principles during the first OpenTofu implementation.
