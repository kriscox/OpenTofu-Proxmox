# ADR-002: Define Deployment Scopes as Composition Boundaries

* Status: Proposed
* Date: 2026-08-18
* Owners: Technical Architect

## Context

ADR-001 defines deployment units as logically coherent architectural capabilities with explicit responsibilities, boundaries, lifecycles and dependencies.

A deployment unit is not necessarily equivalent to the complete functional or platform capability that must be provisioned and operated.

A functional or platform capability may require multiple deployment units to work together. For example, a log aggregation capability may combine several independently defined deployment units while still being understood and deployed as one coherent functional block.

OpenTofu therefore requires an additional composition concept above deployment units.

This concept must:

* allow multiple deployment units to be combined into one functional or platform capability;
* preserve reuse of deployment units;
* avoid duplicating the complete definition for each environment;
* allow limited environment-specific differences;
* provide a future boundary for OpenTofu state and CI/CD orchestration without defining those implementation details yet.

## Decision

A **deployment scope** represents a coherent functional or platform capability that is provisioned and managed using OpenTofu.

A deployment scope primarily composes one or more deployment units.

Deployment units remain independently defined architectural building blocks and do not need to correspond one-to-one with deployment scopes.

A deployment scope may contain a limited amount of scope-specific configuration or infrastructure when this is required to integrate or connect its deployment units and the element does not justify becoming a deployment unit itself.

If such an element develops its own responsibility, lifecycle, reuse potential or independent management requirements, it should be evaluated as a separate deployment unit.

### Environment handling

A deployment scope has one shared definition across environments wherever practical.

Environment-specific configuration may be included within the deployment scope for differences that are relevant only to a specific environment.

Environment-specific configuration should describe only the differences from the shared scope definition rather than duplicate the complete deployment scope.

The deployment scope itself is therefore not tied to a single environment.

### State boundary direction

OpenTofu state is expected to be separated by both deployment scope and environment.

A deployment scope may be instantiated in multiple environments, with separate OpenTofu state maintained for each environment.

The detailed state strategy, backend technology and implementation structure are intentionally deferred to subsequent Phase 3 decisions.

## Alternatives considered

### Deployment unit equals deployment scope

Each deployment unit could directly become an independent OpenTofu deployment scope.

This was rejected because functional capabilities may require several deployment units to operate together and because deployment units were defined primarily as architectural lifecycle and responsibility boundaries rather than OpenTofu composition boundaries.

### One deployment scope per environment

Separate TST, UAT and PRD deployment scopes could be maintained independently.

This was rejected because it would unnecessarily duplicate the functional composition and increase the risk of environments diverging over time.

Environment-specific differences should instead extend a common deployment scope definition.

### Allow unrestricted resources directly in deployment scopes

Deployment scopes could contain arbitrary infrastructure without requiring deployment units.

This was rejected because it would weaken the deployment-unit model and gradually move architectural responsibilities into OpenTofu composition structures.

Scope-specific elements should therefore remain limited to configuration or infrastructure that exists only to compose the deployment units and does not form a meaningful deployment unit itself.

## Consequences

### Positive

* Deployment units remain reusable architectural building blocks.
* Functional and platform capabilities can be represented independently from deployment-unit boundaries.
* Environment definitions can reuse the same functional composition.
* Environment-specific differences remain possible without duplicating complete configurations.
* OpenTofu composition follows the functional architecture without forcing a one-to-one relationship with deployment units.
* The model provides a clear foundation for the later state, module and CI/CD strategies.

### Negative

* An additional architectural concept must be understood and maintained alongside deployment units.
* The boundary between scope-specific configuration and a separate deployment unit requires judgement.
* Environment-specific exceptions can make scopes harder to understand if they become excessive.
* Dependencies between deployment scopes will require explicit management at a higher orchestration level.

## Risks

* Deployment scopes may become too large if unrelated capabilities are grouped under one functional label.
* Deployment scopes may accumulate infrastructure that should instead be promoted to separate deployment units.
* Excessive environment-specific configuration could undermine the intended shared definition.
* Future state or CI/CD requirements may reveal that some deployment scopes need to be split differently.

## Follow-up actions

* [ ] Validate the deployment-scope concept with the other architects.
* [ ] Define the detailed OpenTofu state strategy.
* [ ] Determine the relationship between deployment scopes and OpenTofu modules.
* [ ] Define the environment configuration strategy.
* [ ] Define the repository structure only after the architectural model is sufficiently stable.
* [ ] Define cross-scope dependency and recovery-wave orchestration during the CI/CD phase.
