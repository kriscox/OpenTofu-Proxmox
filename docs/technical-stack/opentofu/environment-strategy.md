# OpenTofu Environment Strategy

## Deployment scope

A deployment scope has one shared OpenTofu implementation across its environments.

Environment-specific configuration must not duplicate or redefine the deployment scope.

## Environment configuration

Environment-specific configuration is co-located with the deployment scope:

```text
ds-<name>/
├── README.md
├── metadata.yaml
├── opentofu/
│   └── ...
└── environments/
    ├── <environment>.tfvars
    └── ...
```

Environment files contain only values that differ from the shared OpenTofu configuration.

Shared defaults belong in the OpenTofu definition where appropriate.

Environment-specific configuration must not contain secrets.

## Lifecycle

Environment configuration follows the lifecycle of its deployment scope.

Removing a deployment scope must therefore also remove its environment-specific configuration.

## Change impact

Changes to shared OpenTofu configuration or shared defaults must be planned against all environments of the deployment scope.

Changes limited to one environment-specific configuration file require planning only against that environment.

The CI/CD implementation must use this distinction when determining the environments affected by a change.

## State

Each deployment scope and environment combination uses an independent OpenTofu state as defined by the state strategy.