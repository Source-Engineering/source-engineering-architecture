# Layer 4 -- Audit Trail Contract

> The interface contract for Layer 4 (Audit Trail). Normative for the discipline's reference implementation, conformance suite, and tools.

This document is the interface contract for Layer 4. It defines what Layer 4 accepts, what Layer 4 produces, and what Layer 4 guarantees. The contract is normative; implementations that claim conformance to this layer must satisfy it.

## What Layer 4 accepts

Layer 4 accepts the DecisionRecord from Layer 1 (post-Layer-2 and Layer-3). Layer 4 also accepts change events: SourceArtifact updates, DecisionRecord appeals, re-runs, corrections, escalations, supersessions.

## What Layer 4 produces

Layer 4 produces:

- A persistent AuditEntry for every change to the DecisionRecord.
- A query interface for re-runs, appeals, and cross-domain comparisons.
- The DecisionRecord's feedback surfaces (`appeal`, `rerun`, `correction`) -- the surfaces the user uses to interact with the audit trail.

## What Layer 4 guarantees

Layer 4 guarantees:

- **Append-only history.** The AuditHistory is append-only. No change to a DecisionRecord is silently lost.
- **Lifecycle integrity.** A change to a SourceArtifact that affects a DecisionRecord is reflected in the DecisionRecord's AuditHistory. The discipline's lifecycle-integrity property is satisfied here.
- **Retention policy.** The retention policy is declared by the domain. The discipline does not impose a default; the domain declares.
- **Re-runnable.** The DecisionRecord can be re-run against the same COTS on a different model or a different version of the same model. The re-run produces a new DecisionRecord linked to the original via the AuditHistory.

## What Layer 4 does not guarantee

- **Persistence guarantees beyond the retention policy.** The discipline does not impose a minimum retention. The domain declares.
- **Tamper-evidence.** The discipline does not require tamper-evidence at Layer 4; the domain can add it.

## Failure modes

Layer 4 fails when:
- The retention policy is missing. The layer returns a `retention-policy-missing` error.
- The persistence backend is unavailable. The layer returns a `persistence-unavailable` error.

## Conformance

A conformance test for this contract lives at `Source-Engineering/source-engineering-conformance/contracts/layer-4/`. The conformance test takes a DecisionRecord and a sequence of change events, invokes the layer, and verifies the layer's guarantees.

## Related documents

- `../four-layer-stack.md` -- the reference architecture.
- `Source-Engineering/source-engineering-model/integrity-questions.md` -- lifecycle integrity.