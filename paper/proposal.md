# Proposal: A Reproducible Study of LLM Package Hallucinations and Public Registry Controls

**Author:** mazdiaz  
**Date:** 2026-09-14  
**Stage:** research proposal / pilot design  
**Target venue:** peer-reviewed cybersecurity or software-engineering venue; final venue to be selected with advisors after pilot feasibility and contribution strength are known

## Abstract

Code-generating large language models (LLMs) sometimes recommend software dependencies that do not exist in public package registries, creating a potential software supply-chain exposure commonly discussed as *slopsquatting*. Prior research has established that package hallucinations occur in Python and JavaScript code generation, and recent 2026 work has expanded into frontier-model replication, cross-model recurrence, registrability, inference-time mitigation, package-hallucination detection, registration census, and agentic coding workflows. These developments make a simple “measure hallucination rates” or “test which names are registrable” study insufficiently differentiated.

This proposal instead studies a narrower measurement problem: **which parts of the hallucinated-name space are covered by package-registry controls that are publicly observable and independently reproducible, what residual candidate exposure remains after those controls, and how stable those findings are across repeated generations and model/registry snapshots.** The study will focus initially on Python/PyPI and JavaScript/npm, use timestamped registry state, distinguish package/distribution names from import/module names, record exact model and prompt configurations, and separate deterministic public controls from governance, post-publication moderation, and undisclosed internal controls. The study will use read-only registry interactions and will not publish or register hallucinated package names. Expected contributions are a reproducible measurement protocol, a characterization of public-control coverage and residual exposure, and evidence about the temporal sensitivity of package-hallucination measurements.

## 1. Introduction

LLM-based coding assistants increasingly recommend libraries, dependencies, and installation commands as part of normal software-development workflows. A reliability and security problem arises when a model recommends a plausible package name that does not exist in the relevant registry. Spracklen et al. (USENIX Security 2025) provide the foundational peer-reviewed measurement of this phenomenon across Python and JavaScript and show that package hallucinations are repeatable rather than purely random one-off outputs.

The security concern is not that every nonexistent package recommendation is automatically exploitable. A meaningful supply-chain incident would require multiple additional conditions, including control of the relevant package name and subsequent trust in the recommendation by a developer or coding agent. Therefore, this proposal uses terms such as **residual candidate exposure** and **residual candidate set** rather than treating every unclaimed name as a confirmed vulnerability.

The research landscape changed materially during 2026. Churilov’s frontier-model replication studies current models, cross-model recurring hallucinations, and names that remain registrable after registry-side review. Djire et al. examine inference-time defenses and show that measurement pipelines can overcount hallucinations when standard-library modules are mishandled. PackMonitor constrains package generation during decoding; BOUND studies model editing; Raj and Sahu propose a package-hallucination detector; Bayesian-calibrated work studies risk probabilities; Okwor studies whether hallucinated names later become registered; and Trend Micro studies agentic/vibe-coding workflows with validation. These works are important even when they are preprints or industry research because they affect novelty and duplication risk.

Accordingly, the July 2026 proposal direction — a broad “Registry Defense Benchmark” plus a supervised evasion classifier — is superseded. The revised project does **not** claim to reconstruct PyPI or npm’s complete security systems. Instead, it evaluates only controls whose behavior can be supported by public documentation and reproduced externally, and it explicitly records the residual candidate set left outside those controls.

### 1.1 Research Gap

Prior work has established package-hallucination prevalence and has begun studying recurrence, registrability, mitigation, detectors, and registration outcomes. Two methodological problems remain especially important for reproducible empirical work:

1. **Registry controls are heterogeneous.** Public name-level constraints, policy/governance rules, post-publication moderation, and undisclosed/internal mechanisms should not be collapsed into a single binary “defense evasion” label.
2. **Registry state is temporal.** A package name can change state over time, so validating old model outputs only against a later live registry can misrepresent the state that existed when the model generated the recommendation.

This study therefore targets a narrower working gap: **reproducible public-control coverage and the temporal stability of the residual hallucinated-name surface**. This positioning is intentionally incremental and must be rechecked against new literature before formal proposal submission and again before paper submission.

## 2. Related Work

A detailed evidence-separated review appears in [`lit_review.md`](lit_review.md), with the closest-work comparison in [`../NOVELTY_MATRIX.md`](../NOVELTY_MATRIX.md).

### 2.1 Foundational Package-Hallucination Measurement

**Spracklen et al. (USENIX Security 2025)** provide the primary peer-reviewed baseline for package hallucinations in code-generating LLMs. Their study spans Python and JavaScript, multiple model families, a large generated-code corpus, repeated hallucinated names, and mitigation strategies. Their methodology also highlights a critical measurement issue for this proposal: module/import names and package/distribution names cannot be treated as equivalent without reconciliation.

### 2.2 Package Confusion and Software Supply-Chain Context

**Neupane et al. (USENIX Security 2023)** develop a broader package-confusion taxonomy showing that dependency-name confusion extends beyond simple edit-distance typos. This taxonomy provides the conceptual basis for RQ4. **Ladisa et al. (IEEE S&P 2023)** provide a broader software supply-chain attack taxonomy and safeguard framework used to situate the threat model. Related peer-reviewed literature in `refs.bib` covers typosquatting, malicious-package detection, install-time defenses, software composition analysis, and supply-chain integrity.

### 2.3 Concurrent 2026 Work Affecting Novelty

The following are treated as **concurrent/preprint evidence unless separately verified as peer-reviewed**:

- **Churilov (2026)** — frontier-model replication, cross-model recurring package names, and registrability after registry-side review. This substantially overlaps the old proposal’s “which names survive defenses?” framing.
- **Djire et al. (2026)** — inference-time defenses across multiple models/languages and a measurement-validity warning about misclassifying standard-library modules.
- **PackMonitor (2026)** — decoding-time monitoring and restriction to authoritative package lists.
- **BOUND (2026)** — package-hallucination mitigation through localized model editing.
- **Raj & Sahu (2026), *Names Can Hurt*** — deterministic registry checking, import-name reconciliation, and a Random Forest risk/detection layer.
- **Bayesian-Calibrated Detection (2026)** — calibrated risk probabilities using registry/metadata signals.
- **Okwor (2026)** — longitudinal/retrospective census of whether hallucinated names become registered.
- **Trend Micro (industry research)** — realistic coding-agent and vibe-coding workflows with validation/MCP-related checks.

These works mean that raw rate replication, generic registrability testing, generic classifier construction, generic RAG/self-refinement, and generic agentic/MCP validation are not sufficient novelty by themselves.

## 3. Research Questions

### RQ1 — Residual exposure

**What proportion of LLM-generated nonexistent package recommendations remain in the residual candidate set after applying publicly observable and independently reproducible package-registry name controls?**

### RQ2 — Characterization

**Which characteristics of hallucinated package recommendations are associated with remaining outside the coverage of those public controls?**

Candidate characteristics include:

- recurrence across repeated generations;
- cross-model agreement;
- lexical/name structure;
- distance or semantic relation to existing packages;
- package-confusion category;
- explicit installation recommendation vs. passive import reference;
- ecosystem;
- prompt family;
- model family/version.

### RQ3 — Stability / reproducibility

**How stable is the residual candidate set across repeated generations, model families or versions, and registry snapshots?**

This RQ is the main reproducibility/temporal differentiator. Depending on advisor approval and project timing, it can be implemented as repeated measurements across several weeks/months or as a smaller robustness design using repeated runs and multiple recorded snapshots.

### RQ4 — Taxonomy

**How are recurring residual candidates distributed across established package-confusion categories, and which categories are least covered by the studied controls?**

RQ4 is secondary characterization rather than the primary novelty claim.

## 4. Operational Definitions

### 4.1 Package Recommendation

A package recommendation is a package/distribution dependency attributable to the model output, supported by one or more of the following evidence types:

1. explicit package-manager installation command in the output;
2. explicit dependency/package list stated by the model;
3. generated dependency manifest;
4. import/require reference that can be reconciled to a package/distribution.

Unresolved module names are retained separately rather than automatically counted as packages.

### 4.2 Package Hallucination

A working operational definition is:

> A package hallucination is a package/distribution recommendation attributable to the model output that, after ecosystem-specific normalization and module-to-distribution reconciliation, is absent from the authoritative registry state corresponding to the recorded observation period.

The implementation must explicitly handle:

- standard-library/built-in modules;
- local/relative modules;
- aliases;
- scoped npm names;
- known import-to-distribution mismatches;
- packages that were deleted, renamed, or appear after generation;
- prohibited/unavailable names;
- ambiguous mappings.

### 4.3 Residual Candidate

A **residual candidate** is a hallucinated package recommendation that remains outside the coverage of the studied **publicly observable and externally reproducible** name-level controls.

A residual candidate is **not** equivalent to a confirmed exploitable package or a demonstrated compromise.

## 5. Methodology

### 5.1 Ecosystems

Initial scope:

- **PyPI / Python**
- **npm / JavaScript**

This preserves comparability with foundational work and keeps the project feasible. Additional ecosystems will only be added if they answer a specific research question rather than serving as novelty decoration.

### 5.2 Model Panel

The full experiment will use a balanced panel rather than every available model. The intended design is:

- 2–3 current commercial/frontier API models;
- 2–3 reproducible open-weight coding/general models.

The pilot will begin with 2–3 models.

For every model run, record:

- provider;
- exact model ID;
- version/snapshot if exposed;
- generation timestamp;
- system prompt;
- user prompt and prompt ID;
- temperature;
- top-p;
- maximum output tokens;
- reasoning mode if exposed;
- repetition index;
- API/client/library version.

Open-weight models provide a reproducibility anchor if proprietary APIs later change.

### 5.3 Prompt and Task Families

The study will use at least two prompt families.

#### Family A — Comparability

A carefully selected subset compatible with Spracklen-style package-hallucination measurement. Its purpose is to validate the pipeline and provide a bridge to prior work.

#### Family B — Realistic Dependency-Seeking Tasks

Developer tasks where external packages are genuinely relevant. Tasks should be curated so dependency recommendation is a natural part of solving the problem rather than an artificial hallucination trap.

#### Optional Family C — Stress Condition

An adversarial or ambiguity-focused condition may be included if useful and advisor-approved. Standard and stress-condition results must be reported separately.

Benchmark names will not be included merely because they are common in code-generation research; each task family must be justified for dependency measurement.

### 5.4 Dependency Extraction and Reconciliation

The pipeline will not use a single raw import parser as ground truth. Evidence will be prioritized as:

1. explicit install commands;
2. explicit dependency/package lists;
3. generated manifests;
4. reconciled imports/requires;
5. unresolved modules stored separately.

Required filters/reconciliation include:

- Python standard library;
- Node.js built-ins;
- local/relative paths;
- aliases;
- virtual/framework-specific modules;
- scoped npm packages;
- ecosystem normalization rules;
- known import-to-distribution mappings.

A manually reviewed sample will be used to estimate extraction quality and identify systematic false positives before the full experiment is launched.

### 5.5 Registry State and Snapshots

For each registry observation/snapshot, retain where practical:

- ecosystem;
- acquisition date/time;
- source endpoint or source artifact;
- checksum/hash;
- package-name count;
- normalization script/version;
- relevant source terms/license note;
- filtering logic.

The analysis must not validate an old model output against a much later live registry and silently assume that later state existed at generation time.

### 5.6 Public-Control Evaluation

The study will first build a control inventory from official registry/operator documentation and classify each control by observability/reproducibility.

| Control class | Example | Publicly documented? | Deterministic externally? | Core empirical evaluation? |
|---|---|---:|---:|---:|
| Existing-name collision | PyPI/npm package name already exists | yes | yes | yes |
| Standard-library conflict | PyPI name conflicts documented in project-name availability guidance | yes | largely | yes, after operational verification |
| Similar/confusable-name restriction | PyPI name-similarity constraints | documented at policy/help level | must be verified | only if reproducible |
| Explicitly unavailable/prohibited names | Registry-reserved/admin-restricted names | partially observable | limited | only where reproducible |
| Quarantine/moderation | PyPI administrative/security review state | concept documented | no | descriptive only |
| npm squatting/dispute policy | governance/policy rule | yes | no | descriptive only |
| Undisclosed anti-abuse systems | internal registry mechanisms | no | no | no |

The final evaluated-control set will contain only controls whose operationalization can be defended from primary documentation or reproducible external behavior. Controls that require administrator judgment remain contextual rather than being converted into synthetic pass/fail labels.

### 5.7 RQ1 Analysis — Coverage and Residual Exposure

For each ecosystem and model/prompt family, report:

- number of package recommendations;
- package-level hallucination rate;
- response-level hallucination rate;
- unique hallucinated names;
- unclaimed-name rate at the recorded snapshot;
- coverage attributable to each evaluated public control;
- combined public-control coverage;
- residual candidate count and rate.

Where useful, report confidence intervals for proportions and stratify by ecosystem/model/prompt family.

### 5.8 RQ2 Analysis — Characteristics of Residual Candidates

RQ2 is a characterization/association question, not a supervised “evasion classifier” task.

Planned analyses include:

- distributions by recurrence tier, model family, ecosystem, prompt family, and evidence type;
- lexical/name-structure summaries;
- nearest-package similarity as a descriptive baseline rather than assumed primary signal;
- package-confusion category distribution;
- cross-model agreement;
- explicit install recommendation vs. lower-confidence dependency evidence.

If sample size supports inferential analysis, use transparent tests/models to estimate association (for example categorical tests or an interpretable multivariable regression). Such analysis will be framed as association, not as a production detector, and leakage will be checked carefully.

### 5.9 RQ3 Analysis — Stability

Possible stability measures:

- Jaccard overlap of residual-name sets across repeated runs;
- persistence frequency across repetitions;
- cross-model recurrence;
- overlap across model versions/snapshots;
- transition rates across registry snapshots;
- persistence of the highest-recurrence candidates;
- sensitivity of aggregate results to registry snapshot choice.

The exact temporal schedule will be locked with advisors after the pilot. If the project cannot support several months of repeated collection, the RQ will be limited to repeated-generation and snapshot-sensitivity robustness rather than overstating a longitudinal design.

### 5.10 RQ4 Analysis — Package-Confusion Taxonomy

Recurring residual candidates will be mapped, where defensible, to Neupane et al.’s established package-confusion categories. The analysis will report:

- category frequency;
- category distribution by ecosystem;
- public-control coverage by category;
- recurrence by category;
- unclassified/ambiguous cases separately.

This analysis is explanatory and taxonomic, not a claim that every hallucination constitutes a documented attack type.

## 6. Threat Model and Non-Claims

The dedicated threat model is maintained in [`../THREAT_MODEL.md`](../THREAT_MODEL.md).

The research studies the early measurement stages of a potential slopsquatting risk chain: model dependency recommendation, package existence at a recorded registry state, recurrence, and coverage by public controls.

The experiment does **not** establish:

- real-world malicious package publication;
- real developer/agent installation behavior;
- code execution in victim systems;
- effectiveness against undisclosed/internal registry controls;
- real-world compromise prevalence.

These boundaries will be visible in the abstract, methods, results discussion, and limitations.

## 7. Ethics and Responsible Research

- No registration of hallucinated package names.
- No publication of harmful packages or code.
- No attempts to circumvent registry protections.
- Use read-only registry queries where possible and respect terms/rate limits.
- Avoid publicly releasing a fresh list of sensitive recurring unclaimed names before disclosure/risk review.
- Prefer aggregate statistics, transformed identifiers, or controlled sharing if exact names would create unnecessary risk.
- If the study discovers a materially important exposed pattern, coordinate with relevant registry/security contacts and advisors before public release.
- Obtain any institutional/advisor ethics approval required by the university.

## 8. Reproducibility

Detailed requirements are maintained in [`../REPRODUCIBILITY.md`](../REPRODUCIBILITY.md).

At minimum, the artifact will record:

- exact model/version identifiers and generation settings;
- prompt IDs and prompt text or reproducible prompt source;
- generation timestamps;
- registry snapshot/validation timestamps;
- snapshot sources and hashes where practical;
- extraction/normalization version;
- import-to-distribution mapping version;
- analysis environment/dependency versions;
- data-release restrictions and the reason for any redaction.

## 9. Limitations

Expected limitations include:

1. **Incomplete view of registry security:** internal/moderation systems are not externally reproducible and therefore cannot be fully evaluated.
2. **Temporal dependence:** registry state changes and proprietary model endpoints may change or disappear.
3. **Package-name resolution ambiguity:** module-to-distribution mapping is imperfect and ecosystem-specific.
4. **Prompt/task representativeness:** any curated benchmark represents only part of real development behavior.
5. **Proprietary model reproducibility:** exact outputs may not be reproducible after provider updates.
6. **Risk vs. exploitation:** residual candidate exposure is not direct evidence of real-world exploitation.
7. **Taxonomy mapping subjectivity:** some hallucinated names may not cleanly fit an established confusion category.
8. **Longitudinal scope:** a true long-term model/registry drift study may be constrained by the project calendar.

## 10. Pilot Study Plan

The pilot exists to test feasibility, not to produce publication-ready headline numbers.

### Initial scale

- 2 ecosystems: PyPI and npm.
- 2–3 models.
- Approximately 50–200 tasks depending on API cost and prompt design.
- Repetitions on a subset to estimate recurrence.
- One frozen/recorded registry state per ecosystem.
- Manual review sample for extraction/reconciliation errors.

### Pilot questions

The pilot should answer:

1. Does the dependency-extraction/reconciliation pipeline work with acceptable error rates?
2. After standard-library/built-in and mapping corrections, are enough true package hallucinations observed for analysis?
3. Which public controls can actually be operationalized reproducibly from primary documentation/external behavior?
4. Is the residual set large enough for RQ2/RQ4 characterization?
5. How variable are repeated generations?
6. What is the API/compute/data-storage cost of the full study?
7. Does the refined design remain meaningfully differentiated from Churilov and other 2026 work?

No preliminary results will be written until this pilot is actually run.

## 11. Timeline

| Period | Goal |
|---|---|
| Month 1 | Fresh literature review, novelty matrix, advisor topic lock, threat model, reproducibility design |
| Month 2 | Build extraction/reconciliation + registry snapshot pipeline; manual validation; pilot |
| Month 3 | Full baseline generation and repeated-prompt experiments |
| Month 4 | Public-control evaluation and temporal/robustness analysis |
| Month 5 | Statistical analysis, figures, limitations, full paper draft |
| Month 6 | Advisor revision, artifact cleanup, venue selection/formatting, submission preparation |
| Months 7–8 if available | Additional robustness work, reviewer/venue revisions, or extended longitudinal snapshots |

Publication/review timing itself is outside the researchers’ control and is not promised within exactly six months.

## 12. Expected Contributions

Subject to implementation and results, the intended contributions are:

1. **Measurement / reproducibility:** a timestamped pipeline and dataset structure for measuring LLM package recommendations against recorded registry state while handling module-to-distribution ambiguity.
2. **Public-control coverage:** an empirical characterization of how publicly observable, reproducible name-level controls cover hallucinated package candidates.
3. **Residual-surface characterization:** analysis of what remains after those controls by ecosystem, recurrence, model agreement, evidence type, and confusion category.
4. **Temporal robustness:** evidence about how sensitive the residual set and aggregate results are to repeated generations, model/version changes, and registry snapshots.
5. **Practical guidance:** evidence-based recommendations about where additional validation may be most useful across model output, registry naming, dependency-resolution, or review workflows.

Only contributions actually implemented and supported by data will appear in the final paper.

## 13. Novelty Statement

The project does **not** claim to be the first study of package hallucination, registrability, package-hallucination mitigation, package-hallucination detection, registration outcomes, or agentic coding workflows.

The current working differentiation is the combination of:

- public-control **reproducibility/observability** as an explicit methodological boundary;
- a timestamped treatment of registry state;
- residual candidate exposure rather than synthetic “defense evasion” labels;
- robustness/stability across repeated generations and registry/model snapshots.

This novelty claim is provisional. It must be revalidated with a date-stamped search before the proposal is formally submitted and again before paper submission.

## 14. Advisor Decisions Requested

Before the full experiment begins, advisors should confirm:

1. whether the four RQs are sufficiently differentiated and appropriately scoped;
2. whether PyPI + npm is the right ecosystem scope;
3. whether RQ3 should include a true multi-month longitudinal component or a smaller robustness design;
4. whether the proposed public-control boundary is scientifically useful and operationally feasible;
5. whether the pilot scale is sufficient before committing to the full model sweep;
6. any university-specific ethics/disclosure requirements.

## 15. References and Evidence Files

- `../refs.bib` — curated peer-reviewed bibliography from the earlier review.
- `../refs_concurrent.bib` — 2026 concurrent/preprint works relevant to novelty.
- `lit_review.md` — evidence-separated annotated review.
- `../NOVELTY_MATRIX.md` — anti-duplication comparison.
- `../THREAT_MODEL.md` — canonical threat model and non-claims.
- `../REPRODUCIBILITY.md` — reproducibility and artifact protocol.
