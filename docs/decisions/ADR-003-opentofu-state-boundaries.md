# ADR-003: Define OpenTofu State Boundaries

* Status: Proposed
* Date: 2026-08-18
* Owners: Technical Architect

## Context

ADR-002 defines deployment scopes as composition boundaries that combine one or more deployment units into a coherent functional or platform capability.

A deployment scope has one shared definition across environments and may be instantiated independently in TST, UAT and PRD.

OpenTofu state defines the management boundary within which OpenTofu tracks and manages infrastructure resources.

The state strategy must therefore provide sufficient isolation between environments and deployment scopes while avoiding unnecessary fragmentation.

The strategy must also acknowledge that deployment scopes may depend on other deployment scopes.

## Decision

Each combination of **deployment scope and environment** shall have one independent OpenTofu state.

For example:

* `scope-a + TST` has its own state;
* `scope-a + UAT` has its own state;
* `scope-a + PRD` has its own state;
* `scope-b + TST` has a separate state from `scope-a + TST`.

The deployment scope therefore represents the primary OpenTofu management boundary, while the environment represents an independent instantiation of that boundary.

Different environments must never share the same OpenTofu state.

Resources belonging to different deployment scopes are not managed within the same state unless the deployment-scope boundaries themselves are reconsidered.

### Cross-scope dependencies

Separating state by deployment scope does not remove dependencies between scopes.

Dependencies between deployment scopes must be explicitly represented as part of the deployment-scope definition.

This dependency information should be machine-readable so that future CI/CD orchestration can determine the required deployment order.

The technical mechanism through which one deployment scope consumes data produced by another deployment scope is intentionally not defined by this decision.

## Alternatives considered

### One state per environment

All deployment scopes within TST, UAT or PRD could share one state.

This was rejected because it would create a large management and failure boundary, increase coupling between unrelated deployment scopes and prevent independent lifecycle management.

### One state per deployment unit and environment

Each deployment unit could have its own state for every environment.

This was rejected as the default because deployment units are architectural building blocks rather than necessarily independent OpenTofu management boundaries.

It would also increase state fragmentation and create additional cross-state dependencies between deployment units that are intentionally composed and managed together within a deployment scope.

### Shared state across environments

Multiple environments could be represented within one deployment-scope state.

This was rejected because TST, UAT and PRD require independent lifecycle, operational and failure boundaries.

## Consequences

### Positive

* TST, UAT and PRD remain independently managed.
* Problems with one state have a limited blast radius.
* Deployment scopes can evolve and be deployed independently.
* State boundaries align with the deployment-scope architecture.
* Unrelated deployment scopes do not need to share state locking or lifecycle.
* The model provides a clear boundary for future CI/CD orchestration.

### Negative

* The number of OpenTofu states increases with the number of deployment scopes and environments.
* Cross-scope dependencies cannot rely on a single OpenTofu dependency graph.
* Deployment order between dependent scopes must be managed explicitly.
* State management and operational governance become more distributed.
* Temporary unavailability of the state backend can prevent OpenTofu deployment
  and recovery operations even though already deployed infrastructure may
  continue to operate.

## Risks

* Excessive numbers of small deployment scopes could lead to unnecessary state fragmentation.
* Cross-scope dependencies may become difficult to understand if they are not explicitly documented.
* Tight data dependencies between scopes could create hidden coupling despite separate state boundaries.
* Future implementation constraints may reveal that some deployment-scope boundaries need to be reconsidered.
* A simultaneous failure of managed infrastructure and the state backend may
  require manual verification or recovery actions before OpenTofu-managed
  recovery can resume.

> This double-failure scenario is accepted as an exceptional operational case and does not justify a runtime-level high-availability requirement for the state backend.

## Follow-up actions

* [X] Define how deployment-scope dependencies are represented as machine-readable metadata.
* [ ] Define how data is exchanged between independently managed deployment scopes.
* [ ] Select and design the OpenTofu state backend.
* [X] Define state access control, locking and recovery requirements.
* [ ] Integrate deployment-scope dependencies into the later CI/CD orchestration design.
