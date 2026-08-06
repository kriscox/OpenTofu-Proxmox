# ADR-001: Define Deployment Units as Architecture Boundaries

* Status: Proposed
* Date: 2026-08-06
* Owners: Technical Architect

## Context

The platform will consist of multiple infrastructure, platform and application capabilities that must be introduced and evolved incrementally.

Deployment units are used to make this evolution manageable, repeatable and reusable. They divide the platform into bounded parts whose changes, operational impact, security controls and responsibilities can be understood and managed independently where possible.

Clear responsibilities, stable interfaces and configurable inputs allow common deployment units to be reused across environments or similar workloads without unnecessarily duplicating architecture or implementation.

This enables the platform to evolve incrementally, limits the impact of changes and failures, promotes consistent reuse, and provides clear boundaries for deployment, validation, ownership, security and recovery.

Without a shared definition of a deployment unit, infrastructure resources, platform services, applications and application components may be grouped inconsistently.

This would make it difficult to establish clear lifecycle, responsibility, security and dependency boundaries.

Deployment units must support modularity across infrastructure, platform and application concerns while avoiding excessive fragmentation and repeated coordination between units and their responsible owners.

## Decision

A deployment unit is defined as:

> A deployment unit is a logically coherent and explicitly demarcated infrastructure, platform or application capability that can be deployed, changed, validated, secured and managed through its own lifecycle under a clearly assigned responsibility. Its boundaries should, where reasonably possible, be enforced through infrastructure, network, compute, runtime, storage, identity or security controls, while its interfaces and dependencies on other deployment units remain explicit.

The following principles apply:

1. **Coherent responsibility**
   Each deployment unit has one clearly defined primary responsibility.

2. **Clear ownership and lifecycle**
   Each deployment unit has an accountable owner or owning role and can, as far as reasonably possible, evolve through its own deployment and operational lifecycle.

3. **Explicit demarcation**
   Boundaries are defined logically, organisationally and operationally, and are technically enforced wherever practical.

   Technical demarcation may include virtual machines, VLANs, network controls, Kubernetes boundaries, storage separation or identity controls.

4. **Security by design**
   A deployment unit contains the security capabilities that logically belong within its responsibility.

   Security capabilities supplied by another deployment unit or shared platform capability are declared as explicit dependencies.

5. **Explicit interfaces and dependencies**
   Dependencies between deployment units are documented and should avoid unnecessary bidirectional coupling or repeated handovers between responsible owners.

6. **Reusable where appropriate**
   Deployment units should expose stable interfaces and configurable inputs so that common capabilities can be reused across environments or similar workloads without duplicating responsibilities or creating unnecessary coupling.

Not every deployment unit must be generic or reusable. Application-specific deployment units may remain specific where this supports clearer responsibilities and simpler lifecycle management.

## Alternatives considered

### Define deployment units only by technical deployability

This option was rejected because deployability alone does not establish coherent responsibility, ownership, security or reuse boundaries.

### Treat the complete platform as one deployment unit

This option was rejected because it would create excessive coupling, prevent independent evolution and limit the reuse of common capabilities.

### Define deployment units only by organisational ownership

This option was rejected because ownership alone does not provide sufficient lifecycle, technical, security or dependency boundaries.

## Consequences

### Positive

* Lifecycle, ownership and security boundaries become explicit.
* Platform and application capabilities can evolve more independently.
* Common capabilities can be reused consistently across environments and similar workloads.
* Technical demarcation supports clearer security and operational responsibility.
* Dependencies and security assumptions cannot remain implicit.
* Routine changes require less coordination between deployment units and responsible owners.

### Negative / trade-offs

* Defining appropriate deployment-unit boundaries requires deliberate architectural analysis.
* Shared capabilities introduce dependencies that must be documented and governed.
* Technical separation may introduce additional infrastructure and operational complexity.
* Reusable units require stable interfaces and conscious configuration design.
* Incorrect boundaries may only become visible during implementation or operation.

## Verification

* [ ] Initial deployment units are identified.
* [ ] Each deployment unit has one primary responsibility and an accountable owner.
* [ ] Logical, organisational, operational and technical boundaries are documented.
* [ ] Security capabilities and dependencies are explicit.
* [ ] Reusable capabilities have stable interfaces and configurable inputs.
* [ ] Boundaries are reviewed for unnecessary coupling and coordination overhead.

## References

* [Architecture Principles](../architecture-principles.md)
* [Platform Overview](../overview.md)
