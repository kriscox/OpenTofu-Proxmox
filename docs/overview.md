# Platform Overview

## Context

Parking.brussels is at the beginning of a digital transformation aimed at gradually internalising more capabilities across its IT landscape. This requires a solid infrastructure foundation that provides reliable, secure and repeatable environments.

Infrastructure and application deployments currently depend, to varying degrees, on manual activities, environment-specific knowledge and implementation choices that originated within a relatively small IT environment. As the infrastructure grows and responsibilities shift between people, a more structured way of working becomes necessary.

Reproducibility, auditability and the retention of organisational knowledge rather than person-dependent knowledge are becoming increasingly important. These needs are further reinforced by evolving regulatory and compliance requirements, including legislation related to cybersecurity, data and cloud services.

A common architectural and operational foundation is therefore required for provisioning infrastructure and deploying platform components and application workloads in a consistent, secure and automated manner.

Local development and test environments are initially provided through Proxmox, while production workloads are expected to run on a dedicated Kubernetes platform.

The approach aims to maintain consistency across environments without assuming that every infrastructure provider must be implemented identically. Provider-specific capabilities may be used where justified, provided that their impact on portability and reversibility is understood.

## Vision

Provide Parking.brussels with a trusted, auditable and version-controlled platform for infrastructure and application deployments.

Through Infrastructure as Code and automated delivery pipelines, the platform enables consistent, reliable and repeatable deployments across environments, resulting in improved availability, stronger security and increased confidence in the internal platforms and teams.

## Objectives

* Deliver consistent and reproducible environments across the complete platform lifecycle.
* Automate infrastructure provisioning, configuration and application deployment wherever practical.
* Ensure all platform changes are version-controlled, traceable and auditable.
* Improve platform availability, security and operational reliability through standardisation and automation.
* Reduce operational complexity by promoting reusable platform components and documented architectural decisions.
* Minimise platform dependencies by favouring open standards, portable technologies and reversible architectural decisions whenever practical.

## Scope

This platform documentation covers the common architectural and operational foundation required to provision infrastructure and deploy platform components and application workloads in a consistent, secure and automated manner.

It includes:

* Platform architecture and design
* Infrastructure provisioning and lifecycle management
* Kubernetes platform deployment and configuration
* Continuous Delivery capabilities for platform components and application workloads
* Platform automation and delivery pipelines
* Security, governance and operational standards
* Platform documentation and Architecture Decision Records

Application source code and application-specific implementation are maintained in their respective repositories.

## Stakeholders

| Stakeholder                                        | Responsibility                                                                                                                                              |
| -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Digital Transformation Manager                     | Provides strategic direction, aligns the platform with the digital transformation objectives and supports prioritisation across organisational initiatives. |
| Business Process Owners and Business Unit Managers | Define business expectations for the applications and services supported by the platform and depend on their availability, security and reliability.        |
| Enterprise Architect                               | Ensures alignment with enterprise architecture principles, organisational standards and the broader target architecture.                                    |
| Technical Architect                                | Defines the technical platform architecture, evaluates design choices and ensures coherence across infrastructure, platform and deployment capabilities.    |
| IT Manager                                         | Provides operational ownership, allocates resources and ensures that the platform aligns with IT priorities and service responsibilities.                   |
| IT Operations                                      | Operates and supports the platform, manages incidents and contributes operational requirements and experience.                                              |
| Security and Compliance                            | Defines applicable security and compliance requirements and supports their translation into architecture, controls and operational practices.               |

## Current Situation

The current infrastructure landscape has evolved within a relatively small IT environment and relies on a combination of manual activities, environment-specific configurations and knowledge held by individual team members.

Infrastructure provisioning, configuration and application deployments are not yet managed through one consistent and fully automated approach. Processes may differ between environments and applications, making deployments harder to reproduce, review and audit.

There are currently no shared guidelines for determining which applications or components should be containerised and how containerised workloads should be designed and operated. Decisions are therefore often made on a case-by-case basis, without a common architectural framework.

Operational, security and reliability measures are frequently introduced in response to immediate needs or incidents rather than being systematically incorporated into the design and delivery process from the beginning.

As the IT landscape grows and responsibilities evolve, this way of working introduces increasing operational risks. Changes can become dependent on specific individuals, documentation may not always reflect the implemented state, and rebuilding or transferring an environment can require significant manual effort.

The current landscape does not yet provide a common platform foundation for consistently managing infrastructure and application deployments across local and external environments.

## Target Situation

Parking.brussels operates a reliable, secure and repeatable platform for provisioning infrastructure and deploying platform components and application workloads.

Infrastructure and deployments are defined as code, maintained under version control and executed through automated delivery pipelines. Changes are reviewable, traceable and reproducible across environments.

The platform provides a consistent architectural and operational model for local and external infrastructure environments. Local development and test environments can be provisioned on Proxmox, while production workloads run on a dedicated Kubernetes platform.

Clear guidelines support decisions on whether applications and components should be containerised and define the standards that containerised workloads must follow. Security, reliability and operational requirements are incorporated into architecture and delivery processes from the beginning rather than introduced only in response to incidents or immediate needs.

The platform is organised into well-defined and understandable modules with clear responsibilities and interfaces. This enables individual components to be maintained and evolved independently while preserving a coherent overall architecture.

Platform knowledge, operational procedures and architectural decisions are maintained as shared organisational knowledge and evolve together with the implementation.

Provider-specific capabilities may be used where they provide justified value, while dependencies and their impact on portability and reversibility remain visible and consciously managed.

## Key Constraints

* The platform must evolve within the capacity of a relatively small internal IT organisation.
* Existing services and operational responsibilities must remain available while the platform is introduced.
* Local development and test environments are initially hosted on the available Proxmox infrastructure.
* Production workloads are expected to run on an external Kubernetes platform, while the final provider and platform choice has not yet been confirmed.
* The architecture must comply with Parking.brussels enterprise architecture, security and compliance requirements.
* The platform must be introduced incrementally; a complete replacement of the existing landscape in a single transition is not feasible.

## Related Documentation

* [Architecture Principles](architecture-principles.md)
