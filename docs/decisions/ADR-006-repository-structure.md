i# ADR-006: Define Repository Structure Strategy

* Status: Proposed
* Date: 2026-08-18
* Owners: Technical Architect

## Context

The platform architecture distinguishes between deployment units, deployment scopes and OpenTofu modules.

These concepts have different responsibilities:

* deployment units represent architectural building blocks;
* deployment scopes compose deployment units into coherent OpenTofu management boundaries;
* OpenTofu modules represent reusable technical implementation capabilities.

The repository structure must preserve these distinctions while remaining simple enough for the current size of the platform.

The structure must also avoid making repository boundaries part of the architecture so that components can later be moved to separate repositories without redesigning their responsibilities or interfaces.

Deployment scopes are defined independently from environments. Environment-specific differences affect implementation and configuration but must not create separate deployment-scope structures.

## Decision

Deployment units, deployment scopes and OpenTofu modules are initially maintained in a single repository.

The repository uses separate top-level structures for each artefact type:

```text
deployment-units/
deployment-scopes/
modules/
```

Repository boundaries are not architectural boundaries.

Components may later be extracted into separate repositories when lifecycle, ownership, scale or operational requirements justify this, without changing their architectural meaning.

### Naming convention

Artefacts use a type prefix followed by a descriptive lowercase kebab-case name.

The prefixes are:

* `du-` — Deployment Unit
* `ds-` — Deployment Scope
* `om-` — OpenTofu Module

Examples:

```text
deployment-units/
  du-log-ingestion-storage/

deployment-scopes/
  ds-observability/

modules/
  om-proxmox-vm/
  om-azure-storage-account/
```

Ordering numbers are not part of the naming convention.

Deployment order, dependency order and recovery priority must be represented explicitly through metadata or orchestration rather than encoded in artefact names.

### Deployment scope structure

Each deployment scope must provide one human-readable `README.md` using a consistent structure.

The README describes:

* purpose;
* scope boundaries;
* composition;
* dependencies;
* constraints;
* decisions and deviations.

Environment-specific information must not be part of the human-readable deployment-scope definition.
Environment-specific differences belong exclusively to the implementation and configuration of the scope.

The README must remain human-oriented and must not duplicate technical configuration that can be derived directly from OpenTofu code or machine-readable configuration.

### Machine-readable deployment-scope metadata

Deployment scopes contain minimal machine-readable metadata for information required by automation and not reliably derivable from OpenTofu itself.

The initial metadata contains:

* a stable deployment-scope identifier;
* explicit dependencies on other deployment scopes.

The metadata model must remain extensible so that additional orchestration metadata, such as recovery-wave information, can be added later when those concepts are formally defined.

Human-readable architectural information must not be duplicated into machine-readable metadata unless automation requires it.

### Environment independence

The repository structure of a deployment scope is environment-independent.

TST, UAT and PRD do not result in separate deployment-scope structures or duplicated scope definitions.

Environment-specific differences belong to the implementation and configuration of the deployment scope and contain only the differences required for that environment.

Adding another environment must not require redesigning or duplicating the deployment-scope structure.

### Templates

Reusable repository templates are maintained under `assets/`.

Templates provide a consistent starting structure for new deployment units, deployment scopes and OpenTofu modules.

Templates are implementation aids and do not define architectural boundaries independently from the corresponding ADRs.

## Alternatives considered

### Separate repositories from the start

Deployment units, deployment scopes and modules could each be maintained in separate repositories.

This was rejected for the initial platform because it would introduce additional repository management, access control, versioning and dependency-management complexity before the platform scale requires it.

The architecture must nevertheless preserve the ability to split repositories later.

### Repository structure per environment

Separate repository structures could be maintained for TST, UAT and PRD.

This was rejected because deployment scopes have one shared definition across environments.

Duplicating structures per environment would increase drift risk and contradict the deployment-scope architecture.

### Ordering prefixes

Numeric prefixes could be added to artefact names to represent deployment or recovery order.

This was rejected because deployment order can change independently from the identity of an artefact.

Ordering and dependency information must therefore remain explicit metadata rather than becoming part of the name.

## Consequences

### Positive

* The repository remains simple to navigate.
* Architectural concepts remain visibly separated.
* Naming remains meaningful when artefacts appear outside their immediate directory context.
* Deployment scopes remain environment-independent.
* Human-readable documentation remains consistent.
* Automation receives explicit machine-readable dependency information.
* Components can later be extracted into separate repositories without redefining their architecture.

### Negative

* Some naming information is redundant inside the corresponding top-level directory.
* A monorepo may require more sophisticated versioning and change-detection mechanisms as the platform grows.
* Maintaining templates introduces an additional artefact that must evolve together with the architecture.

## Risks

* The monorepo may become difficult to manage if platform size or ownership grows significantly.
* Human-readable and machine-readable dependency information may diverge if changes are not maintained consistently.
* Templates may become outdated if architectural decisions evolve without corresponding template updates.
* Environment-specific configuration may grow excessively and undermine the intended shared scope definition.

## Follow-up actions

* [ ] Create deployment-scope documentation and metadata templates under `assets/`.
* [ ] Define the concrete OpenTofu implementation structure inside deployment scopes.
* [ ] Define deployment-unit repository structure.
* [ ] Define OpenTofu module repository structure.
* [ ] Define the module versioning mechanism.
* [ ] Reassess repository boundaries when lifecycle, ownership or scale justify separation.
