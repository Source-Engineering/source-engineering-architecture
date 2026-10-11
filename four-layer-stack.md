# Source Engineering -- Four-Layer Stack (Reference Architecture)

> The canonical reference architecture. Each layer named, scoped, owned.

This document is the canonical reference architecture. It defines the four layers every Source Engineering system composes from, the responsibilities of each layer, the interfaces between layers, and the ownership of each layer. The architecture is normative for the discipline's reference implementation, conformance suite, and tools; the architecture is informative for the discipline's specifications (the specifications say *what* the data must look like; the architecture says *where* the data goes).

## The four layers

```
+--------------------------------------------------------+
|  Layer 4  --  Audit Trail                                |
|  (who saw what, when, and with what evidence)           |
|  Hands: COTS -> AuditRecord                             |
|  Hands: nothing -> (events flow upward only)            |
+--------------------------------------------------------+
                            ^
                            |
+--------------------------------------------------------+
|  Layer 3  --  Business Guardrails                        |
|  (policy, compliance, jurisdictional rules)             |
|  Hands: COTS -> GuardrailDecision                       |
+--------------------------------------------------------+
                            ^
                            |
+--------------------------------------------------------+
|  Layer 2  --  Runtime Validation                         |
|  (calibration, drift, segment-aware signals)            |
|  Hands: Context + DecisionBasis -> Validation           |
+--------------------------------------------------------+
                            ^
                            |
+--------------------------------------------------------+
|  Layer 1  --  Typed Model                                |
|  (typed questions, declared answer shapes,              |
|   calibrated probabilities)                             |
|  Hands: Input -> DecisionRecord                          |
+--------------------------------------------------------+
```

Each layer hands a COTS-shaped artefact to the layer above. The discipline's audit surface is the union of the four layers' outputs.

## Layer 1 -- Typed Model

**Responsibility.** Accept a typed question, return a decision record that conforms to the typed answer shape. The layer does not validate the decision against policy; that is Layer 3's job. The layer does not record the audit trail; that is Layer 4's job.

**Inputs.**
- A typed question that conforms to a registered decision-type (see `Source-Engineering/source-engineering-specifications/decision-types/`).
- The Context for this call (model identifier, model version, parameters).

**Outputs.**
- A DecisionRecord that conforms to the COTS schema (see `Source-Engineering/source-engineering-specifications/cots/`).
- A list of EvidenceItems that link the DecisionRecord back to SourceArtifacts.
- A list of CounterEvidenceItems the layer considered and rejected.

**Guarantees.**
- The DecisionRecord is structured (the answer shape is honoured).
- The EvidenceItems are well-formed (every item has a Provenance and an Uncertainty).
- The CounterEvidenceItems are non-empty when the decision rejects a counter-position.

**Ownership.** Domain maintainer of `source-engineering-architecture` plus domain maintainer of `source-engineering-specifications` (the layer is the bridge).

## Layer 2 -- Runtime Validation

**Responsibility.** Validate the DecisionRecord from Layer 1 against the runtime parameters. Surface calibration drift, segment-aware signals, and parameter-set appropriateness to the user.

**Inputs.**
- The DecisionRecord from Layer 1.
- The Context from the call.
- Calibration data (drift metrics, segment-aware signals, parameter-set benchmarks).

**Outputs.**
- A Validation record attached to the DecisionRecord.
- A list of ValidationFlags (drift detected, segment-signal divergence, parameter-set out-of-range).

**Guarantees.**
- The Validation record is recorded before the DecisionRecord is exposed to Layer 3.
- The ValidationFlags are first-class data points (the user sees them, not just the operator).

**Failure modes.** A ValidationFlag is a warning, not a rejection. The discipline records the flag; the user decides whether to act. Layer 3 is the layer that can reject the decision.

## Layer 3 -- Business Guardrails

**Responsibility.** Apply the domain's policy, compliance, and jurisdictional rules to the DecisionRecord. Decide whether the DecisionRecord is acceptable for the user, or whether it must be rejected, annotated, or escalated.

**Inputs.**
- The DecisionRecord from Layer 1 (post-Layer-2 Validation).
- The domain's guardrail rules.
- The user's declared constraints (jurisdiction, recency window, redaction policy).

**Outputs.**
- A GuardrailDecision (accepted, rejected, annotated, escalated).
- The rationale for the GuardrailDecision.

**Guarantees.**
- The GuardrailDecision is recorded with the DecisionRecord.
- The rationale distinguishes between binding rules (rejection) and advisory rules (annotation).
- An escalated DecisionRecord surfaces to the user with the escalation reason.

**Failure modes.** A rejection by a binding rule stops the response. The discipline does not silently drop the response; the rejection is recorded with the rationale.

## Layer 4 -- Audit Trail

**Responsibility.** Persist the DecisionRecord (with the GuardrailDecision and the Validation record) for the lifetime of the decision. Support appeal, re-run, correction, and supersession.

**Inputs.**
- The DecisionRecord from Layer 1 (post-Layer-2 and Layer-3).

**Outputs.**
- A persistent AuditEntry for every change to the DecisionRecord.
- A query interface for re-runs, appeals, and cross-domain comparisons.

**Guarantees.**
- The AuditHistory is append-only.
- A change to a SourceArtifact that affects a DecisionRecord is reflected in the DecisionRecord's AuditHistory (the lifecycle-integrity property).
- The retention policy is declared by the domain (the discipline does not impose a default).

## Cross-layer composition

The four layers compose into a single response:

```
[User typed question]
        |
        v
[Layer 1 -- Typed Model]
        |
        | DecisionRecord + CounterEvidenceItems
        v
[Layer 2 -- Runtime Validation]
        |
        | DecisionRecord + Validation + ValidationFlags
        v
[Layer 3 -- Business Guardrails]
        |
        | DecisionRecord + GuardrailDecision
        v
[Layer 4 -- Audit Trail]
        |
        | Persistent AuditEntry
        v
[COTS response]
```

The COTS the user receives is the union of all four layers' outputs. The user can re-render any layer's contribution from the COTS alone.

## What the architecture is not

- **Not a deployment topology.** The four layers describe the engineering surface, not where the bytes go. A deployment may run all four on one host, on four hosts, or in any combination; the discipline does not dictate.
- **Not a vendor stack.** The four layers are not a buy recommendation. The reference implementation in `source-engineering-reference-implementation` is one way to compose them.
- **Not a runtime model.** The layers are not stages in time. A decision can be processed through all four layers in a single call or in a sequence of calls.

## Related documents

- `layer-contracts/` -- the per-layer interface contracts.
- `blueprints/v0.1/` -- the first blueprint: the typed-model floor.
- `pillar-x-layer-matrix.md` -- the 5x4 mapping.
- `Source-Engineering/source-engineering-model/model.md` -- the conceptual model the layers implement.
- `Source-Engineering/source-engineering-specifications/cots/README.md` -- the COTS spec the layers produce.