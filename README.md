# LLM Package Hallucinations and Public Registry Controls

Research on package hallucinations in code-generating LLMs, the resulting *slopsquatting* attack surface, and the extent to which **publicly observable and independently reproducible** package-registry controls reduce that exposure.

> **Status:** proposal stage. The July 2026 “Registry Defense Benchmark + evasion classifier” direction has been superseded. The current proposal focuses on reproducible public-control coverage, residual candidate exposure, and temporal robustness.

## Problem

Code-generating LLMs can recommend software dependencies that do not exist in a package registry. If an attacker registers a repeatedly hallucinated name and a developer or coding agent later installs it, the hallucination can become part of a software supply-chain attack path commonly described as *slopsquatting*.

Prior work already establishes that package hallucination is real and 2026 work has expanded into frontier-model replication, registrability, mitigation, package-hallucination detection, registration census, and agentic coding workflows. This project therefore does **not** aim merely to prove that LLMs hallucinate packages or that some names may be registrable.

The working gap is narrower: **how much of a timestamped hallucinated-name corpus is covered by package-registry controls that can be observed and reproduced externally, what residual candidate exposure remains, and how stable are those findings across repeated generations and model/registry snapshots?**

## Research Questions

- **RQ1 — Residual exposure:** What proportion of LLM-generated nonexistent package recommendations remain in the residual candidate set after applying publicly observable and independently reproducible package-registry name controls?
- **RQ2 — Characterization:** Which characteristics of hallucinated package recommendations are associated with remaining outside the coverage of those public controls?
- **RQ3 — Stability / reproducibility:** How stable is the residual candidate set across repeated generations, model families or versions, and registry snapshots?
- **RQ4 — Taxonomy:** How are recurring residual candidates distributed across established package-confusion categories, and which categories are least covered by the studied controls?

RQ4 is a secondary characterization rather than the primary novelty claim.

## Scope

Initial scope:

- **Ecosystems:** PyPI and npm.
- **Languages:** Python and JavaScript for comparability with foundational work and because they map naturally to the selected registries.
- **Models:** a balanced panel of current commercial/frontier APIs and reproducible open-weight models, frozen by exact model ID/version at experiment time.
- **Interactions:** read-only registry queries and frozen/timestamped registry snapshots where practical.

Go/Rust or additional registries will not be added solely to inflate novelty.

## What “public registry controls” means

The study separates four control classes:

1. **Public, name-level controls that are reproducible externally** — eligible for empirical coverage measurement.
2. **Policy/governance rules requiring human or administrator judgment** — described, not converted into deterministic labels without evidence.
3. **Post-publication security controls** such as malware review/quarantine — contextual unless a reproducible public mechanism exists.
4. **Undisclosed/internal controls** — explicitly out of scope.

The project does **not** claim to reproduce complete PyPI/npm security systems or to measure a full internal “defense catch rate.”

## Dependency Evidence and Hallucination Measurement

Raw `import` / `require` parsing is not sufficient because module names and package/distribution names are not always identical. Dependency evidence will be prioritized as follows:

1. explicit package-manager install commands;
2. explicit dependency/package lists stated by the model;
3. generated manifests such as `requirements.txt`, `pyproject.toml`, or `package.json`;
4. import/require references after ecosystem-specific reconciliation;
5. unresolved module names retained separately rather than automatically counted as hallucinations.

The pipeline must also filter standard-library/built-in modules, local/relative modules, aliases, scoped npm names, and known import-to-distribution mismatches.

A package hallucination will be operationalized against the **recorded registry state for the observation period**, after normalization and package/distribution reconciliation. Model generation time, registry snapshot/validation time, and pipeline version will be logged.

## Proposed Analysis

The core study will measure:

- package- and response-level hallucination rates;
- unique and recurring hallucinated names;
- cross-model recurrence;
- coverage of each reproducible public control;
- combined public-control coverage;
- residual candidate counts/rates;
- recurrence and package-confusion categories among residual candidates;
- stability using repeated generations, model/version comparisons, registry snapshots, and overlap/transition metrics.

A supervised “defense-evasion classifier” is **not** part of the core proposal because the previous target label was derived from the same rules that generated the features/labels. A predictive model would require an independent outcome label.

## Evidence Policy

The repository now separates evidence by purpose:

- **Peer-reviewed literature:** primary academic evidence; curated in [`refs.bib`](refs.bib).
- **Concurrent/preprint literature:** used to assess novelty, duplication, and emerging methods; tracked separately in [`refs_concurrent.bib`](refs_concurrent.bib) and the literature review.
- **Industry/operator evidence:** used for registry behavior, policy, operational context, and real-world workflow evidence.

Preprints are not treated as equivalent to peer-reviewed work, but they cannot be ignored when making novelty claims in a fast-moving 2026 field.

## Ethics and Non-Claims

- No registration of hallucinated package names.
- No malicious package publication or payload deployment.
- No attempts to bypass registry security controls.
- Read-only registry interactions where possible; rate limits and terms must be respected.
- Fresh high-value unclaimed recurring names will not be released publicly before disclosure/risk review.
- An unclaimed hallucinated name is a **residual candidate**, not a confirmed exploitable vulnerability.
- The study does not prove malicious registration, victim installation, malware execution, success against undisclosed controls, or real-world compromise prevalence.

See [`THREAT_MODEL.md`](THREAT_MODEL.md) for the full attack chain and [`REPRODUCIBILITY.md`](REPRODUCIBILITY.md) for logging/snapshot requirements.

## Proposal Materials

- [`paper/proposal.md`](paper/proposal.md) — full proposal draft.
- [`paper/lit_review.md`](paper/lit_review.md) — evidence-separated literature review.
- [`NOVELTY_MATRIX.md`](NOVELTY_MATRIX.md) — closest-work comparison and differentiation check.
- [`THREAT_MODEL.md`](THREAT_MODEL.md) — attacker/victim model, measured stages, non-claims.
- [`REPRODUCIBILITY.md`](REPRODUCIBILITY.md) — model, prompt, registry, extraction, and release protocol.
- [`paper/advisor_one_pager.md`](paper/advisor_one_pager.md) — concise advisor-facing summary.
- [`CONTEXT.md`](CONTEXT.md) — current project state for collaborators/AI assistants.

## Timeline

The first **1–2 months** are for literature refresh, novelty lock, advisor approval, pipeline prototyping, and a small pilot. The intended research horizon is approximately **6–8 months**, with full experiments and paper writing occurring only after the exact RQs and pilot design are approved.

## License

MIT — see [LICENSE](LICENSE).
