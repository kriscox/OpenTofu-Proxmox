# OpenTofu Coding Standards

- [Naming](#naming)
- [File organisation](#file-organisation)
- [Variables and outputs](#variables-and-outputs)
- [Sensitive values](#sensitive-values)
- [Formatting and validation](#formatting-and-validation)
- [Comments](#comments)
- [Local values](#local-values)
- [Dependencies and lifecycle](#dependencies-and-lifecycle)
- [Version constraints](#version-constraints)

## Naming

- OpenTofu identifiers use `snake_case`.
- Names must be semantic and must not encode deployment order.
- Platform resource names follow the authoritative naming convention of the target platform.
- Azure resource names and tags follow `ADR-LZ-2026-003`.

## File organisation

Start with the smallest practical structure:

- `main.tf`
- `variables.tf`, when inputs are required
- `outputs.tf`, when outputs are required

Additional files are introduced only when they improve readability or separation of responsibilities.

Empty files must not be created solely for structural consistency.

## Variables and outputs

- Variables are introduced only for values that must be configurable.
- Variables use explicit types whenever practical.
- Validation is added only when it expresses a meaningful constraint not already covered by the type system.
- Defaults are used only when they are safe and meaningful.
- Outputs expose only information required by consumers or orchestration.

## Sensitive values

- Secrets must not be hardcoded or committed to version control.
- Sensitive variables and outputs are marked `sensitive` where appropriate.
- Sensitive values should come from an approved secret-management mechanism.
- `sensitive` does not prevent values from being stored in OpenTofu state.

## Formatting and validation

- Code must conform to `tofu fmt`.
- Configuration must pass `tofu validate`.
- These checks should be automated in CI/CD.

## Comments

- Prefer clear structure and semantic naming over comments.
- Comments explain non-obvious reasoning, constraints or workarounds.
- Comments must not merely restate the code.
- Unresolved design questions and TODOs must not replace documented decisions or tracked follow-up work.

## Local values

- Locals are used for derived values, repeated expressions or readability.
- Consumer-configurable values belong in variables.
- Locals must not hide important configuration or environment-specific settings.

## Dependencies and lifecycle

- Prefer dependencies derived naturally from resource references.
- Use explicit `depends_on` only when the dependency cannot be expressed naturally.
- Lifecycle rules are introduced only for a clear technical or operational reason.

## Version constraints

OpenTofu or provider versions should be constrained when required for compatibility or reproducibility.

Dependency lock files should be committed where applicable.