> Remove all instructional blockquotes and placeholder values when creating a deployment unit.

# <Deployment Unit Name>

## Purpose

> Describe the primary responsibility of this deployment unit.

## Scope

### In scope

- <Responsibility or capability included in this deployment unit>

### Out of scope

- <Responsibility or capability explicitly excluded from this deployment unit>

## Ownership and lifecycle

> Describe the accountable owner or owning role and any relevant lifecycle characteristics.

- **Owner:** <Role or team>
- **Lifecycle:** <Relevant lifecycle information>

## Boundaries

> Describe the logical, organisational, operational and technical boundaries of this deployment unit.
>
> Include the mechanisms that enforce these boundaries where relevant, such as network, compute, runtime, storage, identity or security controls.

- <Boundary>

## Interfaces

> Describe the stable interfaces exposed or consumed by this deployment unit.

| Interface | Direction | Purpose |
|---|---|---|
| <Interface> | Inbound / Outbound | <Purpose> |

## Dependencies

> Describe dependencies on other deployment units or shared platform capabilities.

| Dependency | Reason |
|---|---|
| `du-<name>` | <Why this dependency is required> |

## Constraints

> Describe technical, security, operational or organisational constraints that influence this deployment unit.

- <Constraint>

## Decisions and deviations

> Document deployment-unit-specific decisions and conscious deviations from established architecture principles, standards or conventions.
>
> If no deployment-unit-specific decisions or deviations apply, state:
>
> ***No deployment-unit-specific decisions or deviations.***