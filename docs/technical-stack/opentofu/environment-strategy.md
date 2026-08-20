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