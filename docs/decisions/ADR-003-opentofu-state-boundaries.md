# ADR-003: Define OpenTofu State Boundaries

* Status: Proposed
* Date: 2026-08-18
* Owners: Technical Architect

## Context

Deployment scopes represent coherent functional or platform capabilities with independent lifecycle and management boundaries.

A deployment scope has one shared definition across environments and may be instantiated independently in TST, UAT and PRD.

Where OpenTofu is used as a delivery mechanism, its state defines the management boundary within which OpenTofu tracks and manages resources.

The state strategy must therefore provide sufficient isolation between environments and OpenTofu-managed deployment scopes while avoiding unnecessary fragmentation.

The strategy must also acknowledge that deployment scopes may depend on other deployment scopes, including scopes using different delivery mechanisms.

## Decision

For every deployment scope that uses OpenTofu, each combination of **deployment scope and environment** shall have one independent OpenTofu state.

For example:

* `scope-a + TST` has its own state;
* `scope-a + UAT` has its own state;
* `scope-a + PRD` has its own state;
* `scope-b + TST` has a separate state from `scope-a + TST`.

The deployment scope represents the primary OpenTofu management boundary where OpenTofu is used, while the environment represents an independent instantiation of that boundary.

Different environments must never share the same OpenTofu state.

Resources belonging to different deployment scopes must not be managed within the same OpenTofu state unless the deployment-scope boundaries themselves are reconsidered.

A deployment scope that does not use OpenTofu does not require OpenTofu state.

### Cross-scope dependencies

Separating OpenTofu state by deployment scope does not remove dependencies between scopes.

Dependencies between deployment scopes must remain explicitly represented as part of the deployment-scope definition.

This dependency information should be machine-readable so that CI/CD orchestration can determine the required deployment order.

A dependency does not require both deployment scopes to use OpenTofu or to share the same delivery mechanism.

The technical mechanism through which one deployment scope consumes data or capabilities provided by another deployment scope is not defined by this decision.

## Alternatives considered

### One state per environment

All OpenTofu-managed deployment scopes within TST, UAT or PRD could share one state.

This was rejected because it would create a large management and failure boundary, increase coupling between unrelated deployment scopes and prevent independent lifecycle management.

### One state per deployment unit and environment

Each OpenTofu-managed deployment unit could have its own state for every environment.

This was rejected as the default because deployment units are architectural building blocks rather than necessarily independent OpenTofu management boundaries.

It would also increase state fragmentation and create additional cross-state dependencies between deployment units that are intentionally composed and managed together within a deployment scope.

### Shared state across environments

Multiple environments could be represented within one deployment-scope state.

This was rejected because TST, UAT and PRD require independent lifecycle, operational and failure boundaries.

## Consequences

### Positive

* TST, UAT and PRD remain independently managed where OpenTofu is used.
* Problems with one state have a limited blast radius.
* OpenTofu-managed deployment scopes can evolve independently.
* State boundaries align with deployment-scope lifecycle boundaries.
* Unrelated deployment scopes do not share state locking or lifecycle.
* Deployment scopes that do not use OpenTofu do not require artificial OpenTofu state.
* Cross-scope dependencies can exist independently from the delivery mechanism used by each scope.

### Negative

* The number of OpenTofu states increases with the number of OpenTofu-managed deployment scopes and environments.
* Cross-scope dependencies cannot rely on a single OpenTofu dependency graph.
* Deployment order between dependent scopes must be managed explicitly.
* State management and operational governance become more distributed.
* Temporary unavailability of the state backend can prevent OpenTofu deployment and recovery operations even though already deployed infrastructure may continue to operate.

## Risks

* Excessive numbers of small OpenTofu-managed deployment scopes could lead to unnecessary state fragmentation.
* Cross-scope dependencies may become difficult to understand if they are not explicitly documented.
* Tight dependencies between scopes could create hidden coupling despite separate state boundaries.
* Changes to deployment-scope boundaries may require corresponding changes to OpenTofu state boundaries.
* A simultaneous failure of managed infrastructure and the state backend may require manual verification or recovery actions before OpenTofu-managed recovery can resume.

This double-failure scenario is accepted as an exceptional operational case and does not justify a runtime-level high-availability requirement for the state backend.

## Follow-up actions

* [x] Define how deployment-scope dependencies are represented as machine-readable metadata.
* [ ] Define how data is exchanged between independently managed deployment scopes.
* [ ] Select and design the OpenTofu state backend.
* [x] Define state access control, locking and recovery requirements.
* [ ] Integrate deployment-scope dependencies into CI/CD orchestration.
