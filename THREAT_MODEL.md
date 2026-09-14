# Threat Model

**Last updated:** 2026-09-14

This study uses a deliberately narrow, non-operational threat model. It measures package recommendations produced by LLMs, their recorded registry state, recurrence, and the coverage of public controls that can be reproduced externally.

## Actors

- **LLM / coding assistant:** produces dependency recommendations.
- **Developer or coding agent:** may rely on generated recommendations; actual user behavior is outside the experiment.
- **Package registry:** provides observable package state and public documentation.
- **External party:** represents the general possibility that package ownership/state can change over time; the experiment does not simulate this actor.

## Measured Stages

1. LLM produces a dependency recommendation.
2. The recommendation is normalized and reconciled to a package/distribution name.
3. The package state is evaluated against the recorded registry observation.
4. Publicly documented and externally reproducible controls are evaluated where possible.
5. Remaining names form a `residual candidate set`.
6. Recurrence and stability are compared across repeated generations and recorded snapshots.

## Measured Outcomes

- package recommendations;
- reconciliation status;
- registry existence at a recorded time;
- package-hallucination rate;
- repeated/cross-model recurrence;
- public-control coverage;
- residual candidate count/rate;
- change across repeated runs and snapshots;
- package-confusion category for secondary analysis.

## Explicit Non-Claims

The study does not establish real user behavior, real-world execution, actual incident prevalence, or the behavior of undisclosed/internal registry mechanisms. A residual candidate is therefore an observation for analysis, not a claim of end-to-end impact.

## Trust Boundaries

1. **Model output:** what the LLM actually generated.
2. **Extraction/reconciliation:** how model output is converted into a package/distribution observation.
3. **Registry state:** what was recorded at a specific time.
4. **Public controls:** what can be supported and reproduced from public evidence.
5. **Unobserved mechanisms:** administrative or internal behavior that is outside the measurement scope.

## Terminology

Preferred terms are `package hallucination`, `residual candidate`, `residual candidate set`, `residual candidate exposure`, `publicly observable control`, and `registry snapshot/observation`.

Avoid claims of complete platform effectiveness or end-to-end impact unless separately demonstrated by an approved future study.

## Research Ethics

The study is passive and measurement-focused. It will not publish package names as part of an experiment, will not run harmful code, and will use read-only registry observations where practical. Platform terms, rate limits, and responsible disclosure requirements must be respected.
