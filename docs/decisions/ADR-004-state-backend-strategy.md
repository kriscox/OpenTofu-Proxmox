# ADR-004: Select OpenTofu State Backend Strategy

* Status: Proposed
* Date: 2026-08-18
* Owners: Technical Architect

## Context

ADR-003 defines one independent OpenTofu state for each combination of deployment scope and environment.

The state backend must satisfy the requirements defined in `docs/technical-stack/opentofu/state-backend-requirements.md`, including recoverability, locking, access control, encryption, auditability and portability.

The organisation is currently strongly oriented towards Microsoft Azure and already uses Microsoft Entra ID as a central identity platform.

At the same time, operational autonomy and reversibility remain important architectural concerns. Provider-specific services are acceptable when they provide clear value, provided that migration to another supported solution remains practical.

A state backend must also be usable when OpenTofu is executed outside Azure, including deployments against the local Proxmox environment.

## Decision

**Azure Blob Storage is the preferred OpenTofu state backend.**

The OpenTofu `azurerm` backend provides remote state storage in Azure Blob Storage and supports state locking and consistency checking.

Using Azure Blob Storage does not require the infrastructure managed by OpenTofu to run in Azure. Local Proxmox environments and CI/CD runners can use the same remote backend provided that they have secure network connectivity and appropriate authentication.

Azure Blob Storage is preferred because:

* Azure is already a strategic platform within the organisation;
* it integrates naturally with the existing Microsoft identity and access-control ecosystem;
* it provides a managed storage service without introducing an additional on-premises platform to operate;
* OpenTofu provides a dedicated Azure backend with state locking;
* storage requirements for OpenTofu state are small, making the expected infrastructure cost very low;
* the same backend can support local Proxmox deployments and future Azure-hosted production environments;
* state remains migratable to another OpenTofu-supported backend if the architectural direction changes.

### Alternative 1 — On-premises S3-compatible storage

An on-premises S3-compatible object store is the preferred alternative when greater infrastructure autonomy becomes necessary.

**Garage** is the preferred S3 implementation when a dedicated lightweight object store is required.

Garage is designed specifically as a lightweight, self-hosted S3-compatible object store for small-to-medium deployments and avoids introducing a large storage platform solely for OpenTofu state.

**Ceph Object Gateway** remains a valid alternative when a Ceph cluster already exists as part of the wider platform.

Deploying and operating a Ceph cluster solely for OpenTofu state would introduce disproportionate complexity. However, when Ceph already exists for other storage requirements, using its S3-compatible Object Gateway may be preferable to deploying Garage as an additional storage service.

The OpenTofu S3 backend provides a portable abstraction that also facilitates migration between compatible S3 implementations where required.

### Alternative 2 — PostgreSQL

PostgreSQL is retained as a fallback option.

The OpenTofu PostgreSQL backend supports remote state and state locking and could be hosted on-premises using well-understood database technology.

It is not preferred because OpenTofu state is naturally object-like data and using a relational database introduces database operational responsibilities without providing a clear advantage over Azure Blob Storage or S3-compatible object storage for this use case.

### Rejected — MinIO Community Edition

MinIO was considered because of its historical position as a widely used self-hosted S3-compatible object store.

It is not selected for a new deployment because the upstream community project's current lifecycle and maintenance model introduces an unnecessary long-term maintainability risk compared with actively maintained alternatives.

## Consequences

### Positive

* No additional storage platform is required for the preferred solution.
* State management aligns with the organisation's existing Azure orientation.
* The same backend can be used for both local Proxmox and future Azure deployments.
* Authentication and access control can integrate with the existing identity architecture.
* State backend operations are largely managed by Azure.
* A clear on-premises alternative remains available.
* Migration to another OpenTofu-supported backend remains possible.

### Negative

* The preferred state backend introduces a dependency on Azure availability and connectivity.
* Local deployments require connectivity to Azure to perform OpenTofu state operations.
* The preferred solution reduces infrastructure autonomy compared with fully on-premises storage.
* Azure-specific access-control and operational configuration will be required.

## Risks

* Excessive use of Azure-specific capabilities could make later migration unnecessarily difficult.
* Loss of connectivity to Azure can temporarily block OpenTofu deployment operations.
* If organisational Azure strategy changes, the state backend may need to be migrated.

These risks are accepted because state is not required for the normal runtime of already deployed infrastructure and because state migration to another supported backend remains possible.

## Follow-up actions

* [ ] Validate Azure Blob Storage against all state backend requirements.
* [ ] Define the Azure storage configuration required for state.
* [X] Define the required production and non-production access separation.
* [ ] Validate state migration to an alternative backend during the implementation or recovery testing phase.
