## Naming conventions

OpenTofu identifiers must use `snake_case`.

This applies to:

- resource block names;
- data source block names;
- module block names;
- variables;
- local values;
- outputs.

Names should describe the responsibility or purpose of the object rather than its implementation order.

Numeric ordering prefixes such as `01_`, `02_` or similar must not be used in OpenTofu identifiers.

### Platform resource names

Names assigned to actual platform resources must follow the naming conventions defined by the target platform.

For Azure resources, the authoritative naming and tagging convention is defined in:

`ADR-LZ-2026-003 — Nommage, tagging & organisation des ressources`

OpenTofu code must not introduce an alternative Azure naming convention.

## File organization

OpenTofu configuration should be organised into a small number of files based on responsibility.

The following conventional files should be used where applicable:

- `main.tf` — primary resources and module composition;
- `variables.tf` — input variables;
- `outputs.tf` — outputs;
- `locals.tf` — local values when their number or complexity justifies a separate file;
- `providers.tf` — provider configuration and provider requirements;
- `versions.tf` — OpenTofu and provider version constraints.

Additional `.tf` files may be introduced when they represent a clear technical responsibility and improve readability.

Files must not be split solely by resource type or to keep files artificially short.