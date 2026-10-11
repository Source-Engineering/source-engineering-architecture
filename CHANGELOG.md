# Changelog

All notable changes to the Source Engineering architecture repo are recorded here.

## 2026-10-11

### Added

- `four-layer-stack.md` -- the canonical reference architecture. Four named layers with responsibilities, interfaces, and ownership.
- `pillar-x-layer-matrix.md` -- the 5x4 mapping. Each cell carries a verdict (covered, partial, gap) and a rationale. 9 covered, 6 partial, 3 gap.
- `layer-contracts/layer-1-typed-model.md` -- the Layer 1 interface contract.
- `layer-contracts/layer-2-runtime-validation.md` -- the Layer 2 interface contract.
- `layer-contracts/layer-3-business-guardrails.md` -- the Layer 3 interface contract.
- `layer-contracts/layer-4-audit-trail.md` -- the Layer 4 interface contract.
- `blueprints/v0.1/README.md` -- the first blueprint. Floors the typed-model layer (Layer 1) with a minimum-viable implementation. Names the conformance test cases TC-L1-01 through TC-L1-07.
- `CHANGELOG.md` -- this file.

### Notes

- The four-layer stack is derived from the 2026 reframing of the discipline's five pillars. The mapping is in the pillar x layer matrix.
- The v0.1 blueprint is pre-release. Per the versioning policy, pre-release versions are not required to maintain backwards compatibility.
- The architecture repo owns the canonical blueprint stack. Later blueprints (v0.2, v0.3, ...) supersede v0.1.