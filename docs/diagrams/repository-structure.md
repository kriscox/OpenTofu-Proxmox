# Repository Structure

## Purpose

This diagram visualises the high-level repository structure defined by ADR-006.

```text
OpenTofu-Proxmox/
├── deployment-units/
│   └── du-<name>/
├── deployment-scopes/
│   └── ds-<name>/
│       ├── README.md
│       ├── metadata.yaml
│       └── opentofu/
├── modules/
│   └── om-<name>/
├── docs/
│   ├── decisions/
│   ├── diagrams/
│   └── technical-stack/
└── assets/
    ├── ds-readme-template.md
    └── ds-metadata-template.yaml