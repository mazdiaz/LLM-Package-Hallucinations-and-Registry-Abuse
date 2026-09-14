# Project Context — For Collaborators and AI Assistants

**Last updated:** 2026-09-14  
**Repository:** `mazdiaz/LLM-Package-Hallucinations-and-Registry-Abuse`  
**Stage:** proposal refresh / advisor-review preparation  
**Research horizon:** approximately 6–8 months total; first 1–2 months for proposal, novelty lock, pipeline prototype, and pilot

This file supersedes the July 2026 project context.

## 1. Research Area

The project studies **LLM package hallucinations, slopsquatting risk, and public package-registry controls**. Code-generating LLMs can recommend package/dependency names that do not exist in public registries. The project measures this behavior and the externally observable control surface around it; it does not perform package publication or offensive testing.

## 2. September 2026 Direction — Pivot D2

The July direction — “Registry Defense Benchmark + evasion classifier” — is **superseded**.

Reasons:

- Churilov 2026 overlaps frontier-model replication, cross-model recurring names, and registrability analysis.
- Other 2026 work covers inference-time defenses, constrained decoding, model editing, detectors/risk calibration, registration census, and agentic/MCP workflows.
- Registry anti-abuse systems are not fully public deterministic algorithms that an external researcher can reconstruct.
- The old classifier target was circular unless an independent outcome label could be obtained.

### Current working thesis

> Measure the coverage and limitations of **publicly observable, independently reproducible package-registry controls** over a timestamped corpus of LLM-generated dependency hallucinations, then characterize the **residual candidate exposure** and its stability across repeated generations and model/registry snapshots.

This is a working differentiation, not a priority claim. Novelty must be rechecked before the formal proposal and again before paper submission.

## 3. Research Questions

- **RQ1 — Residual exposure:** What proportion of LLM-generated nonexistent package recommendations remain in the residual candidate set after applying publicly observable and independently reproducible package-registry name controls?
- **RQ2 — Characterization:** Which characteristics of hallucinated package recommendations are associated with remaining outside the coverage of those public controls?
- **RQ3 — Stability / reproducibility:** How stable is the residual candidate set across repeated generations, model families or versions, and registry snapshots?
- **RQ4 — Taxonomy:** How are recurring residual candidates distributed across established package-confusion categories, and which categories are least covered by the studied controls?

RQ4 is secondary analysis rather than the primary novelty claim.

## 4. Scope and Method Constraints

- Initial ecosystems: **PyPI and npm**.
- Initial language alignment: **Python and JavaScript**.
- Do not add ecosystems/languages merely to claim novelty.
- Use a balanced model panel and freeze exact model IDs/versions and generation configuration.
- Registry state is temporal; generation and validation/snapshot times must be recorded.
- Raw `import` / `require` parsing is not sufficient ground truth because module and package/distribution namespaces can differ.
- Handle standard-library/built-in modules, local/relative modules, aliases, scoped names, and known import-to-distribution mismatches explicitly.

### Dependency evidence hierarchy

1. explicit package-manager commands contained in model output;
2. explicit dependency/package lists stated by the model;
3. generated manifests;
4. import/require references after ecosystem-specific reconciliation;
5. unresolved module names retained separately.

### Registry-control classes

1. **Public + externally reproducible name-level controls** — eligible for empirical evaluation.
2. **Policy/governance rules requiring judgment** — descriptive unless a reproducible rule is documented.
3. **Post-publication review/security controls** — contextual unless reproducible externally.
4. **Undisclosed/internal controls** — out of scope.

The project must not claim to reproduce the complete PyPI/npm security stack.

## 5. Evidence Policy

The July rule “peer-reviewed sources only; preprints excluded” is superseded **for novelty assessment**.

### Peer-reviewed literature

Primary academic evidence. `refs.bib` remains the curated peer-reviewed bibliography assembled in the earlier review.

### Concurrent/preprint literature

Used to assess duplication and emerging methods. These sources must be clearly labeled as concurrent/preprint unless independent verification shows peer-reviewed publication.

Key 2026 novelty-relevant items include:

- Aleksandr Churilov — frontier-model replication and recurring names;
- Djire et al. — inference-time defenses and measurement-validity issues;
- PackMonitor — decoding-time package constraints;
- BOUND — model-editing mitigation;
- Raj & Sahu, *Names Can Hurt* — package-hallucination detection and import-name reconciliation;
- Bayesian-calibrated slopsquat detection;
- Daniel Okwor — hallucination-to-registration census.

These are tracked in `refs_concurrent.bib` and `paper/lit_review.md`.

### Industry/operator evidence

Use official registry documentation for registry behavior and policy claims. Industry research can provide operational context, but it must not be silently presented as peer-reviewed evidence.

## 6. Foundational Academic Anchors

1. **Spracklen et al., USENIX Security 2025** — foundational package-hallucination measurement and mitigation study.
2. **Neupane et al., USENIX Security 2023** — package-confusion taxonomy used for secondary characterization.
3. **Ladisa et al., IEEE S&P 2023** — broader open-source software supply-chain taxonomy and safeguards.
4. Supporting literature in `refs.bib` on package confusion, malicious-package detection, install-time defenses, AI code-assistant security, SCA, and supply-chain integrity.

## 7. Current Method Outline

### Model panel

Pilot with 2–3 models first. Expand only after advisor approval. Final panel should balance current commercial/frontier APIs with reproducible open-weight models.

Log provider, exact model ID/version, timestamp, prompts/prompt ID, generation settings, repetition index, and API/library version.

### Prompt families

1. Spracklen-compatible subset for comparability.
2. Realistic developer tasks where external dependencies are naturally relevant.
3. Optional stress condition, kept separate from standard conditions and used only if advisor-approved.

### Registry snapshots

Record acquisition time, source endpoint, checksum/hash where practical, package-name count, normalization version, source terms/license note, and filtering logic.

### Analysis

Core metrics may include package-/response-level hallucination rates, unique and recurring names, cross-model recurrence, public-control coverage, residual candidate count/rate, confusion-category coverage, Jaccard overlap, persistence, and state transitions across snapshots.

RQ2 should use transparent characterization/association analysis. A supervised predictive model is optional only if an independent outcome label later becomes available.

## 8. Threat Model and Non-Claims

See `THREAT_MODEL.md` for the canonical defensive threat model.

The proposed experiment measures generation behavior, registry state at recorded snapshots, recurrence, public-control coverage, residual candidates, and temporal transitions where collected.

It does **not** establish real-world compromise, package publication outcomes, user installation behavior, code execution, or the behavior of undisclosed registry controls.

## 9. Repository Structure After Refresh

```text
LLM-Package-Hallucinations-and-Registry-Abuse/
├── README.md
├── CONTEXT.md
├── NOVELTY_MATRIX.md
├── THREAT_MODEL.md
├── REPRODUCIBILITY.md
├── refs.bib
├── refs_concurrent.bib
├── paper/
│   ├── proposal.md
│   ├── lit_review.md
│   ├── advisor_one_pager.md
│   ├── refs_summary.md
│   ├── non_citable_context.md
│   └── handoff_prompt.md
├── data/
├── scripts/
├── results/
└── docs/superpowers/
    ├── specs/
    └── plans/
```

## 10. Current Status — 2026-09-14

### Retained

- Existing repository scaffold.
- Curated peer-reviewed bibliography in `refs.bib`.
- Foundational literature work from the July review.
- September research handoff and approved D2 redesign.

### Refreshed in this branch

- README and context.
- Proposal positioning/RQs/methodology/timeline/contributions.
- Literature-review evidence hierarchy.
- Novelty matrix.
- Threat model.
- Reproducibility protocol.
- Advisor one-pager.
- AI handoff.

### Still pending after the documentation refresh

- Advisor approval of exact RQs.
- Final model list and prompt sampling plan.
- Pilot data collection.
- Manual dependency-extraction validation sample.
- Experiment-time registry snapshots.
- Full model sweep and final statistical analysis.

No numerical preliminary results should be written before a real pilot exists.

## 11. Immediate Next Actions

1. Give advisors `paper/advisor_one_pager.md`, `paper/proposal.md`, and `NOVELTY_MATRIX.md`.
2. Confirm D2 scope and whether RQ3 should include a true multi-month longitudinal component.
3. Re-read the most critical papers in full where methodology will be reused.
4. Prototype dependency extraction, normalization, and registry snapshot handling.
5. Run a small pilot before committing to the full experiment.
6. Manually validate a sample of extracted dependency recommendations.
7. Re-check the literature immediately before formal proposal submission.
8. Expand only after advisor/pilot approval.

## 12. Instructions for Future AI/Agents

### Always

- Read `paper/proposal.md`, `NOVELTY_MATRIX.md`, `THREAT_MODEL.md`, and `REPRODUCIBILITY.md` before changing methodology.
- Include concurrent/preprint work in novelty checks while keeping evidence classes separate.
- Verify new citation metadata before adding it.
- Distinguish package/distribution names from modules/imports.
- Preserve timestamped registry-state reasoning.
- Keep claims narrow, defensive, and reproducible.

### Do not

- Re-lock the July “Registry Defense Benchmark + evasion classifier.”
- Claim “field wide open,” “first,” or “no prior work” without a fresh date-stamped search.
- Convert undocumented/internal registry behavior into deterministic simulated labels.
- Treat policy language as a published technical detector.
- Treat every unclaimed name as a confirmed vulnerability.
- Add programming languages solely for novelty.
- Start a large API sweep before advisor approval and a small pilot.
