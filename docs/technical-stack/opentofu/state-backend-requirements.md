# State Backend Requirements

## Required capabilities

- State must be stored independently from the machine executing OpenTofu.
- State storage must provide versioning, point-in-time recovery or an equivalent recovery mechanism.
- The backend must support state locking to prevent concurrent write operations against the same state.
- Production state must be access-controlled separately from non-production state.
- The backend must support role-based access control with sufficient granularity to introduce additional separation where required.
- State data must be encrypted at rest.
- State data must be encrypted in transit between OpenTofu and the state backend.
- Access to state data must be auditable.
- State read, write and delete operations must be traceable to an authenticated identity.

## Availability

The state backend does not require runtime-level high availability.

A simultaneous failure of managed infrastructure and the state backend is accepted as an exceptional operational case and may require manual verification or recovery actions.

## Integrity

No additional state-specific integrity-control requirement is defined beyond the existing requirements for access control, auditability, locking and recoverability.

Additional integrity mechanisms may be used when provided by the selected backend, but are not a mandatory selection criterion.

## Portability and autonomy

- Provider-specific state backends are acceptable when they provide clear operational, security or reliability benefits.
- The state backend must allow state to be migrated to another supported backend without requiring reconstruction of the managed infrastructure.
- On-premises state storage should be preferred where practical when it improves operational autonomy without introducing disproportionate complexity or risk.