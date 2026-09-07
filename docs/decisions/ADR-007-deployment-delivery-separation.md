# ADR-007: Separate Deployment Architecture from Delivery Mechanisms

* Status: Proposed
* Date: 2026-09-07
* Owners: Technical Architect

## Context

The platform architecture distinguishes between deployment units, deployment scopes and the mechanisms used to deliver them.

A deployment scope defines a coherent lifecycle and management boundary. The mechanism used to implement that scope may differ depending on the nature of the capability.

Infrastructure-oriented scopes may be best managed with Infrastructure as Code tooling such as OpenTofu.

Kubernetes workloads or packaged platform components may be more appropriately deployed using Helm.

The architectural meaning of a deployment scope must therefore remain independent from the technology used to deploy it.

CI/CD orchestration is a separate concern. It coordinates validation, sequencing, promotion and dependency handling, but does not define the architectural boundaries themselves.

## Decision

Deployment units and deployment scopes are architectural concepts and must not be defined by a specific delivery technology.

A deployment scope may use one or more appropriate delivery mechanisms, including:

* OpenTofu;
* Helm;
* other delivery mechanisms where justified.

No single delivery mechanism is mandatory for every deployment scope.

### Architectural relationship

```text
Architecture
│
├── Deployment Units
│
└── Deployment Scopes
    └── compose Deployment Units
            ↓
      CI/CD orchestration
            ↓
      Delivery mechanisms
      ├── OpenTofu
      ├── Helm
      └── other mechanisms
```

Deployment scopes define what belongs together and which lifecycle boundary applies.

CI/CD orchestration defines when and in which order deployment scopes are validated, deployed or promoted.

Delivery mechanisms define how the desired state is implemented.

### Delivery mechanism selection

The delivery mechanism for a deployment scope must be selected based on technical suitability, lifecycle characteristics and operational responsibility.

Typical examples are:

* infrastructure provisioning → OpenTofu;
* Kubernetes packaged workloads → Helm.

A delivery technology must not be introduced solely to preserve structural uniformity.

### Deployment-scope dependencies

Dependencies between deployment scopes remain architectural dependencies.

CI/CD orchestration may use machine-readable deployment-scope metadata to determine deployment order, validation order or other orchestration requirements.

Dependent deployment scopes do not need to use the same delivery mechanism.

For example:

```text
ds-kubernetes-platform
    delivery: OpenTofu
        ↓
ds-observability
    delivery: Helm
```

The observability scope may depend on capabilities provided by the Kubernetes platform while retaining an independent lifecycle and delivery mechanism.

### State management

State belongs to the delivery mechanism that manages the resources.

A deployment scope does not automatically imply OpenTofu state.

Where OpenTofu is used, OpenTofu state applies only to the resources managed by OpenTofu.

Other delivery mechanisms use their own appropriate deployment or configuration model.

## Alternatives considered

### Require OpenTofu for every deployment scope

Every deployment scope could use OpenTofu as its primary delivery mechanism.

This was rejected because it would force OpenTofu into scenarios where another technology provides a more natural lifecycle model.

### Define deployment scopes as CI/CD concepts

Deployment scopes could be defined directly within CI/CD implementation.

This was rejected because pipeline technology is an implementation concern and may change independently from the platform architecture.

### Define separate architecture concepts per delivery technology

Separate architectural concepts could be introduced for OpenTofu, Helm and other delivery mechanisms.

This was rejected because lifecycle and responsibility boundaries are architectural concerns independent from tooling.

## Consequences

### Positive

* Deployment scopes remain independent from specific tools.
* OpenTofu is used only where it provides meaningful value.
* Kubernetes workloads can use Helm without artificial OpenTofu wrappers.
* CI/CD orchestration can support different delivery mechanisms.
* Delivery tooling can evolve without redefining architecture boundaries.
* Lifecycle and dependency boundaries remain explicit.

### Negative

* Different deployment scopes may use different delivery mechanisms.
* CI/CD orchestration must support multiple deployment workflows.
* Operational teams must understand the lifecycle model of each delivery mechanism.
* Repository structures cannot assume one identical implementation model for every deployment scope.

## Risks

* Excessive variation in delivery mechanisms could reduce operational consistency.
* Tool selection could become inconsistent without clear technical standards.
* Architectural dependencies and orchestration dependencies may diverge if metadata is not maintained correctly.
* Supporting multiple delivery mechanisms may increase orchestration complexity.

These risks should be mitigated through technical standards, reusable delivery patterns and explicit deployment-scope metadata rather than by forcing all deployment scopes to use the same tool.
