# ADR-002: Define Deployment Scopes as Composition Boundaries

* Status: Proposed
* Date: 2026-08-18
* Owners: Technical Architect

## Context

Deployment units represent logically coherent architectural capabilities with explicit responsibilities, boundaries, lifecycles and dependencies.

A deployment unit is not necessarily equivalent to the complete functional or platform capability that must be deployed and operated.

A functional or platform capability may require multiple deployment units to work together while still being understood and managed as one coherent functional block.

An additional composition concept is therefore required above deployment units.

This concept must:

* allow multiple deployment units to be combined into one functional or platform capability;
* preserve reuse of deployment units;
* provide a coherent lifecycle and management boundary;
* avoid duplicating the complete definition for each environment;
* allow limited environment-specific differences;
* support explicit dependencies between independently managed capabilities;
* remain independent from the technical delivery mechanism used to implement the capability.

## Decision

A **deployment scope** represents a coherent functional or platform capability with a common lifecycle and management boundary.

A deployment scope primarily composes one or more deployment units.

Deployment units remain independently defined architectural building blocks and do not need to correspond one-to-one with deployment scopes.

A deployment scope may contain a limited amount of scope-specific configuration or implementation when this is required to integrate or connect its deployment units and the element does not justify becoming a deployment unit itself.

If such an element develops its own responsibility, lifecycle, reuse potential or independent management requirements, it should be evaluated as a separate deployment unit.

### Delivery mechanism independence

A deployment scope is independent from the mechanism used to implement or deploy it.

Different deployment scopes may use different delivery mechanisms depending on their technical characteristics and lifecycle requirements.

The selected delivery mechanism does not change the architectural identity or boundaries of the deployment scope.

A deployment scope may therefore remain the same even when its underlying delivery technology changes.

### Environment handling

A deployment scope has one shared definition across environments wherever practical.

Environment-specific configuration may be included within the deployment scope for differences that are relevant only to a specific environment.

Environment-specific configuration should describe only the differences from the shared scope definition rather than duplicate the complete deployment scope.

The deployment scope itself is therefore not tied to a single environment.

Adding another environment must not require redefining the architectural scope.

### Lifecycle boundary

A deployment scope represents the primary lifecycle and management boundary for the capability it contains.

Changes within a scope should be manageable independently from unrelated scopes wherever dependencies allow.

A deployment scope may be validated, deployed, upgraded or recovered independently while respecting its explicit dependencies on other scopes.

The lifecycle boundary does not require all elements inside the scope to use the same technical implementation mechanism, but they must collectively form one coherent managed capability.

### Dependencies between deployment scopes

Deployment scopes may depend on capabilities provided by other deployment scopes.

These dependencies must be explicit.

Dependencies should describe the capability required from another scope rather than create implicit coupling to its internal implementation.

A dependency between scopes does not require both scopes to share the same lifecycle or delivery mechanism.

## Alternatives considered

### Deployment unit equals deployment scope

Each deployment unit could directly become an independent deployment scope.

This was rejected because functional capabilities may require several deployment units to operate together and because deployment units primarily represent architectural responsibility and lifecycle boundaries of reusable building blocks rather than complete composition boundaries.

### One deployment scope per environment

Separate TST, UAT and PRD deployment scopes could be maintained independently.

This was rejected because it would unnecessarily duplicate the functional composition and increase the risk of environments diverging over time.

Environment-specific differences should instead extend a common deployment-scope definition.

### Allow unrestricted implementation directly in deployment scopes

Deployment scopes could contain arbitrary infrastructure or application implementation without requiring deployment units.

This was rejected because it would weaken the deployment-unit model and gradually move architectural responsibilities into composition structures.

Scope-specific elements should therefore remain limited to implementation required to compose the deployment units and that does not form a meaningful deployment unit itself.

### Define deployment scopes by delivery technology

Separate scope types could be introduced for infrastructure, Helm-based workloads or other delivery mechanisms.

This was rejected because the architectural boundary of a capability must remain independent from the tool used to implement it.

## Consequences

### Positive

* Deployment units remain reusable architectural building blocks.
* Functional and platform capabilities can be represented independently from deployment-unit boundaries.
* Lifecycle and management boundaries remain explicit.
* Deployment scopes remain independent from delivery technology.
* Different delivery mechanisms can be used without changing the architecture model.
* Environment definitions can reuse the same functional composition.
* Environment-specific differences remain possible without duplicating complete configurations.
* Dependencies between capabilities remain explicit.

### Negative

* An additional architectural concept must be understood and maintained alongside deployment units.
* The boundary between scope-specific implementation and a separate deployment unit requires judgement.
* Environment-specific exceptions can make scopes harder to understand if they become excessive.
* Different scopes may require different operational and delivery mechanisms.
* Dependencies between scopes require explicit orchestration and lifecycle coordination.

## Risks

* Deployment scopes may become too large if unrelated capabilities are grouped under one functional label.
* Deployment scopes may accumulate implementation that should instead become separate deployment units.
* Excessive environment-specific configuration could undermine the intended shared definition.
* Delivery mechanisms may become inconsistent if technical standards are not maintained.
* Hidden dependencies may develop if required capabilities between scopes are not documented explicitly.

## Follow-up actions

* [ ] Validate deployment-scope boundaries against real platform capabilities.
* [ ] Maintain explicit dependencies between deployment scopes.
* [ ] Ensure repository templates support environment-independent deployment scopes.
* [ ] Ensure delivery mechanisms remain implementation choices rather than architectural boundaries.
* [ ] Define orchestration of dependencies between deployment scopes.
