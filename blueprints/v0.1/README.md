# Architecture Blueprint v0.1 -- Typed-Model Floor

> The first blueprint. Floors the typed-model layer with a minimum-viable implementation.

This blueprint is the operational form of the architecture at v0.1. It names the specific interface contracts and the specific conformance tests the discipline's reference implementation must satisfy at this version. The blueprint is superseded by later versions; v0.2 (forthcoming) extends the runtime-validation layer.

## Scope of v0.1

v0.1 floors the typed-model layer (Layer 1) with a minimum-viable implementation. The other three layers are referenced but not floored; their full blueprints land in v0.2 and later.

The v0.1 floor is:
- A typed-question interface contract (Layer 1's `accepts`).
- A typed-answer shape contract (Layer 1's `produces`).
- A minimum evidence-anchoring rule (Layer 1's `guarantees.evidence-anchored`).
- A minimum counter-evidence rule (Layer 1's `guarantees.counter-evidence`).
- A context-recording rule (Layer 1's `guarantees.context-recorded`).

## What the v0.1 floor accepts

The v0.1 floor accepts a typed question that conforms to one of the registered decision-types in `Source-Engineering/source-engineering-specifications/decision-types/`. The first registered decision-type is `legal-summarization` (see `Source-Engineering/source-engineering-specifications/decision-types/legal/legal-summarization.md`). The second is `transaction-routing` (see `Source-Engineering/source-engineering-specifications/decision-types/finance/transaction-routing.md`).

A new decision-type can be added to the v0.1 floor via an RFC in `Source-Engineering/source-engineering-governance/RFC/`. The new decision-type lands under the appropriate sub-registry; the floor does not change.

## What the v0.1 floor produces

The v0.1 floor produces a DecisionRecord that:

- Conforms to the COTS schema at v0.1.
- Carries at least one EvidenceItem per Claim.
- Carries at least one CounterEvidenceItem when the decision rejects a counter-position.
- Records the Context for the call.

## What the v0.1 floor guarantees

The v0.1 floor guarantees the four `bulleted` guarantees from `../layer-contracts/layer-1-typed-model.md`:

- Structured output.
- Evidence-anchored.
- Counter-evidence considered.
- Context recorded.

The v0.1 floor does not guarantee the four `bulleted`-**not** guarantees from the same document:

- Correctness.
- Calibration.
- Policy compliance.
- Persistence.

These are the responsibilities of Layers 2, 3, and 4, whose blueprints land in v0.2 and later.

## Conformance test cases

The v0.1 floor must satisfy the following conformance test cases. The test cases are normative; the conformance suite at `Source-Engineering/source-engineering-conformance` runs them.

- **TC-L1-01.** A typed question that conforms to the registered decision-type's question schema is accepted by Layer 1.
- **TC-L1-02.** A typed question that does not conform to the registered decision-type's question schema is rejected with `question-schema-violation`.
- **TC-L1-03.** The model's answer conforms to the registered decision-type's answer schema.
- **TC-L1-04.** Every Claim in the DecisionRecord has at least one EvidenceItem.
- **TC-L1-05.** Every EvidenceItem has a Provenance that resolves to a SourceArtifact.
- **TC-L1-06.** When the model rejects a counter-position, the CounterEvidenceItems are non-empty.
- **TC-L1-07.** The DecisionRecord's `cots.context` records the runtime parameters.

## What v0.1 does not floor

- The runtime-validation layer (Layer 2) is referenced but not floored. The conformance test TC-L2-01 (validation record attached to the DecisionRecord) is informative for v0.1; it becomes normative in v0.2.
- The business-guardrails layer (Layer 3) is referenced but not floored. The guardrail DSL is not yet declared; the conformance tests for Layer 3 are deferred to v0.3.
- The audit-trail layer (Layer 4) is referenced but not floored. The persistence backend is not yet declared; the conformance tests for Layer 4 are deferred to v0.4.

## Versioning of the blueprint

The blueprint is at v0.1 (initial pre-release). Per the versioning policy in `Source-Engineering/source-engineering-specifications/versioning.md`, pre-release versions are not required to maintain backwards compatibility. v0.1 is the seed; v1.0 will be the stable release.

## Related documents

- `../four-layer-stack.md` -- the reference architecture.
- `../layer-contracts/layer-1-typed-model.md` -- the Layer 1 contract.
- `../pillar-x-layer-matrix.md` -- the 5x4 mapping.
- `Source-Engineering/source-engineering-specifications/cots/README.md` -- the COTS schema.
- `Source-Engineering/source-engineering-specifications/decision-types/` -- the registered decision-types.