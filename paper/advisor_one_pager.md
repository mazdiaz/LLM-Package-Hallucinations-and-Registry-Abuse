# Advisor One-Pager

**Working title:** *A Reproducible Study of LLM Package Hallucinations and Public Registry Controls*  
**Date:** 2026-09-14

## Proposed Direction

The July proposal has been narrowed after reviewing 2026 concurrent work. The revised study focuses on **reproducible public-control coverage, timestamped registry observations, residual candidate characterization, and stability across repeated model/registry observations**.

## Research Questions

1. **Residual exposure:** What proportion of LLM-generated nonexistent package recommendations remain after the studied public controls are applied?
2. **Characterization:** Which characteristics are associated with remaining in that residual set?
3. **Stability:** How stable is the residual set across repeated generations, model families/versions, and registry snapshots?
4. **Taxonomy:** How are recurring residual candidates distributed across established package-confusion categories?

RQ4 is secondary.

## Proposed Scope

- PyPI/Python and npm/JavaScript.
- Pilot with 2–3 models before expansion.
- Spracklen-compatible tasks for comparability plus realistic dependency-seeking tasks.
- Timestamped registry observations with versioned extraction/normalization.
- Module/import names reconciled to package/distribution names rather than treated as equivalent.

## Why This Is Different from the July Plan

Recent work now covers current-model replication, recurring names, package-state comparison, inference-time methods, model editing, package-related classification/scoring, temporal state studies, and coding-agent workflows. The revised proposal therefore avoids using a generic classifier or broad platform-effectiveness claim as its central contribution.

## Intended Contribution

Subject to pilot feasibility, the project aims to provide:

- a reproducible measurement protocol;
- public-control coverage estimates;
- characterization of residual candidates;
- stability/snapshot-sensitivity analysis;
- practical recommendations derived from the measured gaps.

## Feasibility

- public data/documentation;
- laptop plus free cloud/API resources;
- no model fine-tuning required;
- pilot before the full experiment;
- approximately 6–8 months total, with the first 1–2 months focused on proposal approval and pilot work.

## Decisions Requested

1. Approve or revise the four RQs.
2. Confirm the PyPI/Python and npm/JavaScript scope.
3. Decide whether RQ3 should use a true multi-month design or a smaller repeated-run/snapshot robustness design.
4. Approve the pilot before the full model sweep.
5. Identify any additional institutional requirements for the study.

## Supporting Files

- `proposal.md` — full proposal.
- `lit_review.md` — updated literature review.
- `../NOVELTY_MATRIX.md` — closest-work comparison entry point.
- `../THREAT_MODEL.md` — research boundaries and non-claims.
- `../REPRODUCIBILITY.md` — metadata and artifact protocol.
