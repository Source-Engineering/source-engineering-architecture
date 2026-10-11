# Layer 3 -- Business Guardrails Contract

> The interface contract for Layer 3 (Business Guardrails). Normative for the discipline's reference implementation, conformance suite, and tools.

This document is the interface contract for Layer 3. It defines what Layer 3 accepts, what Layer 3 produces, and what Layer 3 guarantees. The contract is normative; implementations that claim conformance to this layer must satisfy it.

## What Layer 3 accepts

Layer 3 accepts the DecisionRecord from Layer 1 (post-Layer-2 Validation). Layer 3 also accepts the domain's guardrail rules (the DSL is declared in `Source-Engineering/source-engineering-architecture/blueprints/v0.1/`; the rules themselves live with the domain) and the user's declared constraints (jurisdiction, recency window, redaction policy).

## What Layer 3 produces

Layer 3 produces a GuardrailDecision attached to the DecisionRecord. The GuardrailDecision carries:

- The decision (accepted, rejected, annotated, escalated).
- The rationale for the decision (in domain terms; which rule was applied, what triggered the decision).
- A distinction between binding rules (rejection) and advisory rules (annotation).

Layer 3 also produces the DecisionRecord with the GuardrailDecision attached. The DecisionRecord's `cots.decisions[*].validation_status` is updated to reflect the GuardrailDecision.

## What Layer 3 guarantees

Layer 3 guarantees:

- **Recording with rationale.** Every GuardrailDecision is recorded with the rule that triggered it.
- **Distinction between binding and advisory.** The GuardrailDecision records whether the rule is binding (the decision is rejected) or advisory (the decision is annotated). The user sees the distinction.
- **No silent rejection.** A rejection by a binding rule stops the response; the rejection is recorded with the rationale. The discipline does not silently drop the response.
- **Escalation surfaces.** An escalated DecisionRecord surfaces to the user with the escalation reason.

## What Layer 3 does not guarantee

- **Correctness of the rules.** Layer 3 applies the rules the domain provides; it does not validate the rules themselves.
- **Coverage of the rule space.** The discipline does not assert that every domain has a complete rule set. Gaps are tracked in `Source-Engineering/source-engineering-research/agenda.md`.

## Failure modes

Layer 3 fails when:
- The guardrail rules are malformed. The layer returns a `guardrail-rules-malformed` error.
- The domain's rules are missing. The layer returns a `domain-rules-missing` error.

## Conformance

A conformance test for this contract lives at `Source-Engineering/source-engineering-conformance/contracts/layer-3/`. The conformance test takes a DecisionRecord and a domain's guardrail rules, invokes the layer, and verifies the layer's guarantees.

## Related documents

- `../four-layer-stack.md` -- the reference architecture.
- `Source-Engineering/source-engineering-architecture/blueprints/v0.1/` -- the v0.1 blueprint (the guardrail DSL).