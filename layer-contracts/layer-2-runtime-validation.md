# Layer 2 -- Runtime Validation Contract

> The interface contract for Layer 2 (Runtime Validation). Normative for the discipline's reference implementation, conformance suite, and tools.

This document is the interface contract for Layer 2. It defines what Layer 2 accepts, what Layer 2 produces, and what Layer 2 guarantees. The contract is normative; implementations that claim conformance to this layer must satisfy it.

## What Layer 2 accepts

Layer 2 accepts the DecisionRecord from Layer 1 and the Context for the call. Layer 2 also accepts the calibration data: drift metrics, segment-aware signals, parameter-set benchmarks, and the temporal validity window against which the DecisionRecord is evaluated.

The calibration data is provided by the operator. Layer 2 does not synthesise calibration data.

## What Layer 2 produces

Layer 2 produces a Validation record attached to the DecisionRecord. The Validation record carries:

- A list of ValidationFlags (drift detected, segment-signal divergence, parameter-set out-of-range, temporal-validity-expired).
- The Validation's `decision_on_validation` (passed, flagged, escalated).

Layer 2 also produces the DecisionRecord with the Validation attached. The DecisionRecord's `cots.decisions[*].validation_status` is updated to reflect the Validation.

## What Layer 2 guarantees

Layer 2 guarantees:

- **First-class flags.** ValidationFlags are first-class data points in the DecisionRecord. The user sees them, not just the operator.
- **Pre-Layer-3 recording.** The Validation record is recorded before the DecisionRecord is exposed to Layer 3. The discipline does not allow a decision to be guarded without first being validated.
- **No silent rejection.** A ValidationFlag is a warning, not a rejection. The discipline records the flag; the user decides whether to act.

## What Layer 2 does not guarantee

- **Rejection of invalid decisions.** Layer 2 does not reject; it flags. Layer 3 is the layer that can reject.
- **Calibration of the underlying model.** Layer 2 validates the calibration signals; it does not calibrate the model.

## Failure modes

Layer 2 fails when:
- The calibration data is missing. The layer returns a `calibration-data-missing` error.
- The DecisionRecord does not carry a Context. The layer returns a `context-missing` error.

## Conformance

A conformance test for this contract lives at `Source-Engineering/source-engineering-conformance/contracts/layer-2/`. The conformance test takes a DecisionRecord and calibration data, invokes the layer, and verifies the layer's guarantees.

## Related documents

- `../four-layer-stack.md` -- the reference architecture.
- `Source-Engineering/source-engineering-model/integrity-questions.md` -- contextual integrity.
- `Source-Engineering/source-engineering-specifications/cots/README.md` -- the COTS schema.