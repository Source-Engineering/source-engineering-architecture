# Layer 1 -- Typed Model Contract

> The interface contract for Layer 1 (Typed Model). Normative for the discipline's reference implementation, conformance suite, and tools.

This document is the interface contract for Layer 1. It defines what Layer 1 accepts, what Layer 1 produces, and what Layer 1 guarantees. The contract is normative; implementations that claim conformance to this layer must satisfy it.

## What Layer 1 accepts

Layer 1 accepts a typed question that conforms to a decision-type registered in `Source-Engineering/source-engineering-specifications/decision-types/`. The typed question is one of:

- A JSON object that conforms to the registered decision-type's question schema.
- A wrapper that names the decision-type and carries the question (used when the question schema itself does not include the decision-type identifier).

Layer 1 also accepts the Context for the call (model identifier, model version, parameters). The Context is provided by the caller; Layer 1 does not synthesise it.

## What Layer 1 produces

Layer 1 produces a DecisionRecord that conforms to the COTS schema in `Source-Engineering/source-engineering-specifications/cots/README.md`. The DecisionRecord carries:

- The DecisionRecord's `decision_text` (the model's answer in domain terms).
- The EvidenceItems that support the DecisionRecord, each with a Provenance and an Uncertainty.
- The CounterEvidenceItems that the layer considered and rejected.
- The links to the SourceArtifacts in the Source Universe.

Layer 1 also produces a list of ValidationFlags for Layer 2. The flags are advisory; Layer 1 does not reject decisions on its own.

## What Layer 1 guarantees

Layer 1 guarantees:

- **Structured output.** The DecisionRecord conforms to the answer shape declared by the registered decision-type.
- **Evidence-anchored.** Every Claim in the DecisionRecord has at least one EvidenceItem. Every EvidenceItem has a Provenance. Every Provenance resolves to a SourceArtifact.
- **Counter-evidence considered.** When the model rejects a counter-position, the CounterEvidenceItems are non-empty and recorded.
- **Context recorded.** The Context for the call is in the DecisionRecord's `cots.context`.

## What Layer 1 does not guarantee

- **Correctness.** Layer 1 does not assert that the DecisionRecord is correct. The discipline records the decision; the user reviews it.
- **Calibration.** Layer 1 does not validate the model's calibration; that is Layer 2's job.
- **Policy compliance.** Layer 1 does not apply the domain's policy; that is Layer 3's job.
- **Persistence.** Layer 1 does not persist the DecisionRecord; that is Layer 4's job.

## Failure modes

Layer 1 fails when:
- The typed question does not conform to the registered decision-type's schema. The layer returns a `question-schema-violation` error.
- The model's answer does not conform to the registered decision-type's answer schema. The layer returns an `answer-schema-violation` error.
- The model cannot answer the question with the declared shape (the answer is genuinely unknown). The layer returns an `unknown-answer` indicator.

## Conformance

A conformance test for this contract lives at `Source-Engineering/source-engineering-conformance/contracts/layer-1/`. The conformance test takes a typed question and a model, invokes the layer, and verifies the layer's guarantees.

## Related documents

- `../four-layer-stack.md` -- the reference architecture.
- `../pillar-x-layer-matrix.md` -- the 5x4 mapping.
- `Source-Engineering/source-engineering-specifications/cots/README.md` -- the COTS schema.
- `Source-Engineering/source-engineering-specifications/decision-types/` -- the registered decision-types.