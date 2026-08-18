> Remove all instructional blockquotes and placeholder values when creating a deployment scope.

# <Deployment Scope Name>

## Purpose

> Describe the functional or platform capability provided by this deployment scope and its primary responsibility.

## Scope

### In scope

- <Responsibility or capability included in this deployment scope>

### Out of scope

- <Responsibility or capability explicitly excluded from this deployment scope>

## Composition

> Describe the deployment units that together form this deployment scope and any limited scope-specific integration required between them.
 
| Deployment Unit | Role                                |
| --------------- | ----------------------------------- |
| `du-<name>`     | <Role within this deployment scope> |

## Dependencies

> Describe dependencies on other deployment scopes or platform capabilities.
>
> Only dependencies required for this deployment scope to function should be listed here.

| Dependency  | Reason                            |
| ----------- | --------------------------------- |
| `ds-<name>` | <Why this dependency is required> |

## Constraints

>Describe technical, security, operational or organisational constraints that influence the implementation of this deployment scope.

- <Constraint>

## Decisions and deviations

>Document scope-specific implementation decisions and conscious deviations from established architecture principles, standards or conventions.
>
>If no scope-specific decisions or deviations apply, state:
>
> ***No scope-specific decisions or deviations.***
