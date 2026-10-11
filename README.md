# Source Engineering -- Architecture

> Reference architecture, blueprint stack, per-layer interface contracts.

This repository holds the discipline's reference architecture. It defines the four-layer stack (Typed Model, Runtime Validation, Business Guardrails, Audit Trail) that composes the discipline into a shippable engineering surface. The architecture is the bridge between the model (which says *what the discipline thinks about*) and the specifications (which say *what the discipline's contracts are*).

## What is in this repository

- `four-layer-stack.md` -- the canonical reference architecture. Each layer named, scoped, owned.
- `layer-contracts/` -- the per-layer interface contracts. What each layer accepts, what each layer produces, what each layer guarantees.
- `blueprints/` -- the architecture blueprint stack. Each blueprint versions the architecture for a specific release; v0.1 is the typed-model floor.
- `pillar-x-layer-matrix.md` -- the load-bearing 5x4 mapping. Each pillar maps to one or more layers; each cell carries a verdict (covered, partial, gap) and a rationale.
- `diagrams/` -- Mermaid source files for the official architecture diagrams.

## How to engage

- **Read the four-layer stack first.** Start at `four-layer-stack.md`.
- **Then the per-layer contracts.** `layer-contracts/` is normative for what each layer accepts and produces.
- **Then the pillar x layer matrix.** `pillar-x-layer-matrix.md` is the bridge to the foundations.
- **Then the blueprints.** `blueprints/v0.1/` is the first versioned architecture; later versions supersede it.

## Provenance

The four-layer stack is derived from the 2026 reframing of the discipline's five pillars. The mapping is captured in the pillar x layer matrix. The blueprints are the operational form of the architecture.

## Organisation

The discipline is published as a set of repositories under the [Source-Engineering](https://github.com/Source-Engineering) GitHub organisation. One authoritative home per artefact:

| Repo | Owns |
|---|---|
| [Source-Engineering/.github](https://github.com/Source-Engineering/.github) | Org profile README |
| [Source-Engineering/project-governance](https://github.com/Source-Engineering/project-governance) | Project-level governance for the governance home |
| [Source-Engineering/source-engineering-governance](https://github.com/Source-Engineering/source-engineering-governance) | Org-wide discipline governance |
| [Source-Engineering/source-engineering-foundations](https://github.com/Source-Engineering/source-engineering-foundations) | Discipline definition, principles, terminology |
| [Source-Engineering/source-engineering-research](https://github.com/Source-Engineering/source-engineering-research) | Research agenda, experimental protocols |
| [Source-Engineering/source-engineering-model](https://github.com/Source-Engineering/source-engineering-model) | Canonical conceptual and data model |
| [Source-Engineering/source-engineering-specifications](https://github.com/Source-Engineering/source-engineering-specifications) | Normative technical requirements; COTS spec |
| [Source-Engineering/source-engineering-architecture](https://github.com/Source-Engineering/source-engineering-architecture) (this repo) | Reference architecture, blueprint stack, per-layer interface contracts |
| [Source-Engineering/source-engineering-reference-implementation](https://github.com/Source-Engineering/source-engineering-reference-implementation) | Working reference code |
| [Source-Engineering/source-engineering-conformance](https://github.com/Source-Engineering/source-engineering-conformance) | Conformance test suite |
| [Source-Engineering/source-engineering-tools](https://github.com/Source-Engineering/source-engineering-tools) | Reusable software utilities |
| [Source-Engineering/source-engineering-case-studies](https://github.com/Source-Engineering/source-engineering-case-studies) | Evaluated demonstrations |
| [Source-Engineering/source-engineering-practice](https://github.com/Source-Engineering/source-engineering-practice) | Practitioner guidance |
| [Source-Engineering/source-engineering-education](https://github.com/Source-Engineering/source-engineering-education) | Curriculum and learning resources |

## Status

This is the architecture slice (Phase 2). The four-layer stack, the per-layer contracts, the pillar x layer matrix, and the first blueprint (v0.1, the typed-model floor) are seeded.

## License

Apache-2.0. See [LICENSE](LICENSE).