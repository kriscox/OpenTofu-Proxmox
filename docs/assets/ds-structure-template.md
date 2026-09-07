# Deployment Scope Structure Template

Use this structure as the starting point for a new deployment scope.

```text
ds-<name>/
├── README.md
├── metadata.yaml
├── opentofu/        # optional when OpenTofu is used
├── helm/            # optional when Helm is used
└── environments/    # when environment-specific configuration is required
```

## Templates

- `README.md` → [`ds-readme-template.md`](./ds-readme-template.md)
- `metadata.yaml` → [`ds-metadata-template.yaml`](./ds-metadata-template.yaml)

No separate template is required for the OpenTofu files or environment files.

## Notes

- Create only the delivery-specific directories that are required by the deployment scope.
- Use `opentofu/` for OpenTofu implementation.
- Use `helm/` for Helm-specific configuration.
- Use `environments/` only when environment-specific overrides are required.
- Environment-specific configuration must not contain secrets.

See [`environment-strategy.md`](../technical-stack/opentofu/environment-strategy.md) for the environment rules.
