# Handoff Prompt for New AI / Agent Sessions

Paste the block below into a fresh session when continuing this research project.

---

I am working on an undergraduate cybersecurity/software-engineering research project on **LLM package hallucinations and public package-registry controls**.

Repository: `mazdiaz/LLM-Package-Hallucinations-and-Registry-Abuse`

## Read First

Read these files in order:

1. `CONTEXT.md` — current project state and constraints.
2. `paper/proposal.md` — current full proposal.
3. `paper/lit_review.md` — literature landscape and working gap.
4. `NOVELTY_MATRIX.md` — related-work comparison entry point.
5. `THREAT_MODEL.md` — measured stages and non-claims.
6. `REPRODUCIBILITY.md` — model/prompt/registry/provenance requirements.
7. `paper/advisor_one_pager.md` — advisor-facing summary.

## Current Direction

The **July 2026 “Registry Defense Benchmark + evasion classifier” is superseded**.

The current working direction (Pivot D2) is:

> Measure the coverage and limitations of **publicly observable, independently reproducible package-registry controls** over a timestamped corpus of LLM-generated dependency hallucinations; characterize the **residual candidate set** and its stability across repeated generations and model/registry snapshots.

This is a working differentiation, not a permanent “first” claim.

## Current RQs

- **RQ1 — Residual exposure:** What proportion of LLM-generated nonexistent package recommendations remain in the residual candidate set after applying publicly observable and independently reproducible package-registry name controls?
- **RQ2 — Characterization:** Which characteristics of hallucinated package recommendations are associated with remaining outside the coverage of those public controls?
- **RQ3 — Stability / reproducibility:** How stable is the residual candidate set across repeated generations, model families or versions, and registry snapshots?
- **RQ4 — Taxonomy:** How are recurring residual candidates distributed across established package-confusion categories, and which categories are least covered by the studied controls?

RQ4 is secondary.

## Evidence Rules

- `refs.bib` is the curated peer-reviewed bibliography from the earlier review.
- Concurrent/preprint work **must be considered for novelty**, but must not be silently described as peer-reviewed.
- Industry/operator evidence is appropriate for registry behavior and deployment context.
- Recheck publication status and novelty before formal proposal submission.
- Prefer primary academic or official operator sources.

Important 2026 overlap includes Churilov, Djire et al., PackMonitor, BOUND, Raj & Sahu, Bayesian-calibrated package analysis, Okwor's temporal registry-state work, and Trend Micro's coding-agent study.

## Method Constraints

- Initial ecosystems: PyPI/Python and npm/JavaScript.
- Do not add languages solely for novelty.
- Raw `import` / `require` parsing is not sufficient ground truth.
- Preserve dependency evidence type and reconcile module/import names to package/distribution names.
- Filter standard-library/built-in/local references.
- Record exact model/version/configuration and generation timestamp.
- Record registry observation/snapshot timestamp, source, and processing version.
- Only include externally reproducible public controls in the core coverage metric.
- Do not invent deterministic behavior for undocumented/internal platform mechanisms.
- Do not require a supervised classifier unless an independent target label becomes available.

## Research Boundaries

This project is passive and measurement-focused. Keep real-world user behavior and unobserved platform mechanisms outside the empirical claims. Use the terminology in `THREAT_MODEL.md` and preserve the release/provenance requirements in `REPRODUCIBILITY.md`.

## Current Next Steps

1. Advisor review of `paper/advisor_one_pager.md`, `paper/proposal.md`, and the related-work comparison.
2. Decide whether RQ3 is truly longitudinal or a smaller repeated-run/snapshot robustness design.
3. Re-read the core methodology papers in full.
4. Prototype dependency extraction/reconciliation and registry snapshot handling.
5. Run a small pilot before any large model sweep.
6. Manually validate a sample of extracted dependencies.
7. Repeat novelty search immediately before formal proposal submission.

## Do Not Reintroduce

- “field wide open” as an unsupported claim;
- “first peer-reviewed measurement” without a fresh systematic search;
- the July classifier as a mandatory RQ;
- full-registry-effectiveness wording when only public controls are observed;
- a 1–2 month timeline as if it were the entire research project;
- final numerical results before real data exists.

When continuing work, preserve the current RQs and constraints unless the user/advisors explicitly approve a change.
