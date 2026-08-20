# Deployment Scope Structure Template

Use this structure as the starting point for a new deployment scope.

```text
ds-<name>/
├── README.md                       → ds-readme-template.md
├── metadata.yaml                  → ds-metadata-template.yaml
├── opentofu/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
└── environments/
    ├── <environment>.tfvars
    └── ...
```

## Templates

- `README.md` → [`ds-readme-template.md`](./ds-readme-template.md)
- `metadata.yaml` → [`ds-metadata-template.yaml`](./ds-metadata-template.yaml)

No separate template is required for the OpenTofu files or environment files.

## Notes

- Remove files that are not required.
- Additional OpenTofu files may be added when they improve readability or separation of responsibilities.
- Environment files contain only environment-specific overrides.
- Shared defaults belong in the OpenTofu definition where appropriate.
- Environment-specific configuration must not contain secrets.

See [`environment-strategy.md`](../technical-stack/opentofu/environment-strategy.md) for the environment rules.
