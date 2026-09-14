# Literature Review: LLM Package Hallucination Measurement

**Last updated:** 2026-09-14

## Evidence classes

This review uses three evidence classes. Peer-reviewed publications are the primary academic basis. Concurrent/preprint work is tracked separately because it can affect novelty even before peer review. Registry and industry documentation is used for platform behavior and operational context.

The earlier rule that excluded preprints from novelty checking is therefore superseded.

## Peer-reviewed foundations

### Spracklen et al. (USENIX Security 2025)

Spracklen et al. provide the main peer-reviewed baseline for package hallucination in code-generating LLMs. The study covers Python and JavaScript, multiple model families, repeated outputs, and mitigation experiments. For this proposal, the most important methodological lesson is that module/import names and package/distribution names are not guaranteed to match. The revised pipeline therefore requires explicit reconciliation rather than treating every unresolved import as a package recommendation.

The current project should not use the existence of package hallucination itself as the main contribution because that phenomenon is already established.

### Neupane et al. (USENIX Security 2023)

Neupane et al. provide the package-confusion taxonomy used for the proposal's secondary RQ4 analysis. Their work also supports treating simple edit-distance features as descriptive baselines rather than assuming that close spelling explains all confusing package relationships.

### Ladisa et al. (IEEE S&P 2023)

Ladisa et al. provide broad open-source software supply-chain taxonomy and safeguard context. This source is used for general framing and for distinguishing the package-name layer from other software-supply-chain layers.

### Adjacent peer-reviewed work

The curated `refs.bib` also contains literature on package naming, package-ecosystem analysis, dependency controls, software composition analysis, SBOM/CI/CD, supply-chain integrity, AI-assisted programming, and code hallucination/uncertainty. Together these studies show that multiple layers of validation exist and should not be merged into one binary variable.

## Concurrent work affecting novelty

The items below are treated as concurrent/preprint work unless their status is independently reverified before submission.

### Churilov 2026 — arXiv:2605.17062

Studies 2026 frontier models, Python/JavaScript package hallucination, recurring names across models, and registry-state availability. This overlaps the earlier proposal's broad registry-benchmark direction and means that cross-model recurrence or simple availability measurement is not enough differentiation by itself.

### Djire et al. 2026 — arXiv:2608.22652

Studies several inference-time methods across multiple models/languages and highlights measurement errors caused by standard-library handling. This directly motivates explicit built-in/standard-library filtering and package/distribution reconciliation in the revised method.

### PackMonitor 2026 — arXiv:2602.20717

Studies package-validity constraints during generation. This means generation-time package validation is already an active research direction and is not selected as the primary contribution here.

### BOUND 2026 — arXiv:2607.02052

Studies model editing for package-validity behavior. This direction is active and is also less aligned with the intended undergraduate measurement study.

### Raj and Sahu 2026 — arXiv:2608.23897

Studies package-name checking, import-name reconciliation, engineered features, and supervised classification. This weakens the earlier proposal's classifier contribution while reinforcing the need for correct import/package mapping.

### Hillah, Richard, and Hasnaoui 2026 — arXiv:2606.13918

Studies calibrated package-related scoring using registry/package metadata. Generic metadata scoring is therefore not treated as an untouched direction.

### Okwor 2026

Studies a frozen registry starting point and later state comparison for hallucinated names. This supports the revised proposal's emphasis on timestamped registry state and shows that a registry-state census alone is not an untouched question.

## Industry and registry documentation

Official PyPI and npm documentation should be the basis for statements about registry policy or externally visible behavior. Industry research such as Trend Micro's coding-agent study is useful for deployment context but should not be silently presented as peer-reviewed academic evidence.

## Cross-cutting lessons

1. **Module names and distribution names require reconciliation.**
2. **Registry state is temporal.** Record model-generation time, registry-observation time, source, and pipeline version.
3. **Availability is not equivalent to demonstrated real-world impact.** The revised proposal uses the term `residual candidate exposure`.
4. **Registry controls are heterogeneous.** Separate reproducible public name/state constraints from policy, administrative review, and undisclosed mechanisms.
5. **A classifier is not required.** Predictive work should only return if an independent target label and a clearly distinct research question become available.

## Crowded directions

The following can still be used as baselines or context, but are not strong standalone novelty claims for this project:

- current-model hallucination-rate replication alone;
- extra languages solely for novelty;
- generic retrieval/self-refinement mitigation;
- generation constrained to known package lists;
- model editing for package validity;
- generic package-hallucination classification;
- metadata-based package scoring;
- registry availability alone;
- registry-state census alone;
- generic agentic/MCP validation;
- cross-model recurring names alone;
- package-confusion taxonomy mapping alone.

## Revised working gap

The proposal's current differentiation is:

> A reproducible evaluation of publicly observable package-registry name controls over a timestamped LLM package-hallucination corpus, coupled with characterization of the residual candidate exposure and its stability across repeated generations and registry/model snapshots.

This is intentionally narrower than the July proposal. It does not claim complete registry-security effectiveness, does not invent labels for undocumented mechanisms, does not require a classifier, and treats registry ground truth as temporal.

This remains a working gap. A date-stamped literature search must be repeated before advisor sign-off and before publication submission.

## Relationship to the revised RQs

| RQ | Literature basis | Working differentiation |
|---|---|---|
| RQ1 residual exposure | Spracklen + registry documentation + Churilov overlap | coverage limited to reproducible public controls |
| RQ2 characterization | Spracklen + Neupane + 2026 classification work | characterize residual candidates without a circular classifier |
| RQ3 stability | recurrence + 2026 registry-state work | repeated generations, model versions, and registry snapshots |
| RQ4 taxonomy | Neupane taxonomy | secondary analysis of residual category coverage |

## Next literature work

1. Re-read Spracklen and Neupane in full for exact methodology details.
2. Re-check the latest Churilov version/repository.
3. Review Djire et al.'s exact filtering and measurement definitions.
4. Confirm publication status for every 2026 concurrent item.
5. Re-check current PyPI/npm primary documentation before operationalizing controls.
6. Update `../NOVELTY_MATRIX.md` whenever closely related work appears.
7. Repeat the novelty search immediately before formal proposal submission.
