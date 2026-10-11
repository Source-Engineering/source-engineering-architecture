# Source Engineering -- Pillar x Layer Cross-Matrix

> The load-bearing 5x4 mapping. Each pillar maps to one or more layers; each cell carries a verdict and a rationale.

This document is the discipline's bridge between the five pillars (foundations) and the four-layer stack (architecture). It captures where each pillar is implemented in the architecture, where the pillar's claims are satisfied, and where the gaps are. The matrix is normative for the discipline's roadmap; gaps are tracked as open questions in `Source-Engineering/source-engineering-research/agenda.md`.

## The matrix

|                                          | Layer 1 (Typed Model) | Layer 2 (Runtime Validation) | Layer 3 (Business Guardrails) | Layer 4 (Audit Trail) |
|------------------------------------------|-----------------------|-------------------------------|-------------------------------|----------------------|
| **Pillar 1 -- Data Provenance**          | **Covered.** The Source Universe is the input to Layer 1. The SourceArtifact identifier is the entry point. | **Partial.** Layer 2 records the context that captured the provenance, but does not validate that the provenance is current. | **Gap.** Layer 3 can require a minimum provenance quality, but the discipline does not define the quality levels. | **Covered.** Layer 4's AuditHistory records every change to the provenance chain. |
| **Pillar 2 -- Contextual Signature**     | **Partial.** Layer 1 records the Context as a first-class data point. | **Covered.** Layer 2 validates that the recorded context is appropriate for the decision. | **Partial.** Layer 3 can constrain the context for a domain, but the discipline does not define the constraints. | **Covered.** Layer 4 records the context with the decision. |
| **Pillar 3 -- Chain of Evidence**        | **Covered.** Layer 1 produces EvidenceItems. | **Covered.** Layer 2 validates the EvidenceItems against the recorded context. | **Partial.** Layer 3 requires evidence for binding rules, but does not define the evidence quality. | **Covered.** Layer 4 records the evidence chain for re-evaluation. |
| **Pillar 4 -- Explainability Anchoring** | **Covered.** Layer 1 surfaces the DecisionBasis as part of the answer. | **Covered.** Layer 2 surfaces the ValidationFlags as part of the answer. | **Partial.** Layer 3 surfaces the GuardrailDecision as part of the answer. | **Covered.** Layer 4 surfaces the AuditHistory as part of the answer. |
| **Pillar 5 -- Human Validation**        | **Gap.** Layer 1 does not produce a feedback surface; the feedback surface is a Layer 4 concern. | **Gap.** Layer 2 does not validate the feedback surface. | **Partial.** Layer 3 surfaces the GuardrailDecision to the user, which is one form of feedback. | **Covered.** Layer 4 produces the appeal, rerun, and correction surfaces. |

## Per-cell rationales

### Pillar 1 (Data Provenance) at Layer 1 -- Covered

Layer 1 accepts a typed question that names the SourceArtifacts. The Source Universe is the entry point; every DecisionRecord carries the SourceArtifact identifiers back to the user. The pillar's claim (provenance is a property of the response) is satisfied at the data layer.

### Pillar 1 at Layer 2 -- Partial

Layer 2 validates the context that captured the provenance. It does not validate that the provenance is current; that is a check on the TemporalValidity of the SourceArtifact, which Layer 1 already records. The gap is that Layer 2 does not surface a flag when the provenance has expired; the COTS still records the decision, but the user must check the temporal validity themselves.

### Pillar 1 at Layer 3 -- Gap

Layer 3 applies domain-specific rules. A domain may require that every SourceArtifact be authoritative (e.g. a regulator's published list, not a third-party scrape). The discipline does not define the levels of provenance quality or the rules a domain can apply. The gap is closed by the domain's guardrail rules, not by the discipline.

### Pillar 1 at Layer 4 -- Covered

Layer 4 records every change to a SourceArtifact that affects a DecisionRecord. The lifecycle-integrity property is satisfied here.

### Pillar 2 (Contextual Signature) at Layer 1 -- Partial

Layer 1 records the Context as a first-class data point (the runtime parameters are in the COTS). The pillar's claim (context is a runtime signal) is partially satisfied: the user sees the context, but the discipline does not yet define the contract for what "surfaced to the user" means. The gap is closed by the conformance suite's TC-CI-01 (context recorded).

### Pillar 2 at Layer 2 -- Covered

Layer 2 validates the recorded context against the calibration data. The pillar's claim (context is a runtime signal) is fully satisfied at Layer 2.

### Pillar 2 at Layer 3 -- Partial

Layer 3 applies the domain's constraints on the context (e.g. a regulated domain may require temperature <= 0.3). The discipline does not define the constraints; the domain does.

### Pillar 2 at Layer 4 -- Covered

Layer 4 records the context with the decision. The pillar's claim (the user sees what the model sees) is satisfied.

### Pillar 3 (Chain of Evidence) at Layer 1 -- Covered

Layer 1 produces EvidenceItems. Every Claim in the DecisionRecord has at least one EvidenceItem with a Provenance. The pillar's claim (the trace is evidence-anchored) is satisfied.

### Pillar 3 at Layer 2 -- Covered

Layer 2 validates the EvidenceItems against the recorded context (e.g. the SourceArtifact was valid at the decision's timestamp). The pillar's claim (the trace is inspectable) is satisfied.

### Pillar 3 at Layer 3 -- Partial

Layer 3 can require evidence for binding rules. The discipline does not define the evidence quality; the domain does.

### Pillar 3 at Layer 4 -- Covered

Layer 4 records the evidence chain for re-evaluation. The pillar's claim (the trace is reconstructable) is satisfied.

### Pillar 4 (Explainability Anchoring) at Layer 1 -- Covered

Layer 1 surfaces the DecisionBasis (supporting evidence, counterevidence, constraints, known gaps) as part of the answer. The pillar's claim (the journey is explained in domain terms) is satisfied.

### Pillar 4 at Layer 2 -- Covered

Layer 2 surfaces the ValidationFlags as part of the answer. The pillar's claim (the runtime signals are surfaced) is satisfied.

### Pillar 4 at Layer 3 -- Partial

Layer 3 surfaces the GuardrailDecision. The pillar's claim (the user understands the decision) is partially satisfied; the discipline does not yet specify how the GuardrailDecision is rendered to the user.

### Pillar 4 at Layer 4 -- Covered

Layer 4 surfaces the AuditHistory. The pillar's claim (the user can audit the decision over time) is satisfied.

### Pillar 5 (Human Validation) at Layer 1 -- Gap

Layer 1 does not produce a feedback surface. The pillar's claim (feedback surfaces are designed for the user) is a Layer 4 concern.

### Pillar 5 at Layer 2 -- Gap

Layer 2 does not validate the feedback surface. The gap is closed by Layer 4's production of the appeal, rerun, and correction surfaces.

### Pillar 5 at Layer 3 -- Partial

Layer 3 surfaces the GuardrailDecision to the user. This is one form of feedback (the user knows whether the decision is acceptable). The pillar's claim (feedback surfaces are evidence-aware) is partially satisfied; the discipline does not yet define how the GuardrailDecision's rationale is rendered to the user.

### Pillar 5 at Layer 4 -- Covered

Layer 4 produces the appeal, rerun, and correction surfaces. The pillar's claim (the user can challenge the decision) is satisfied.

## Verdict summary

| Verdict | Count |
|---|---|
| Covered | 9 |
| Partial | 6 |
| Gap | 3 |

The 3 gaps and 6 partials are tracked as open questions in `Source-Engineering/source-engineering-research/agenda.md`. The discipline grows by closing the gaps, not by accumulating epics.

## Related documents

- `four-layer-stack.md` -- the canonical reference architecture.
- `layer-contracts/` -- the per-layer interface contracts.
- `Source-Engineering/source-engineering-foundations/docs/00-manifesto.md` -- the manifesto's three principles.
- `Source-Engineering/source-engineering-research/agenda.md` -- the discipline's open questions.