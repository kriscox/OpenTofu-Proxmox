# Technical Architecture Principles

## Purpose

These technical architecture principles translate the higher-level platform architecture principles into more concrete technical rules.

They guide implementation and technical design decisions while remaining independent from specific implementation technologies wherever practical.

The principles in this document complement the [Architecture Principles](architecture-principles.md).

## Configuration Must Be Version Controlled

Infrastructure configuration, environment-specific configuration and deployment metadata must be maintained under version control.

Version history must provide traceability of infrastructure changes and support controlled restoration of a previous intended configuration.

Version-controlled configuration represents the normal basis for reverting infrastructure changes.

## State Must Be Recoverable

OpenTofu state must be stored independently from the machine executing OpenTofu.

State storage must provide versioning, point-in-time recovery or an equivalent recovery mechanism so that state can be recovered after corruption, accidental modification or operational failure.

Restoring a previous state version is a state-recovery mechanism and must not be used as the normal method for reverting infrastructure changes.
