# Proposal-Ready Reorganization Design

**Date:** 2026-09-14  
**Branch:** `proposal-refresh-2026-09`  
**Stage:** Approved design for proposal refresh

## 1. Goal

Reorganize the repository so it reflects the September 2026 research landscape and is suitable for advisor review as an undergraduate cybersecurity research proposal. The repository must stop presenting the July 2026 "Registry Defense Benchmark + evasion classifier" as a locked novelty claim.

The revised project will study **publicly observable and independently reproducible registry/name controls**, the **residual candidate exposure** that remains after those controls, and the **stability of that residual set** across repeated generations and registry/model snapshots.

## 2. Scientific Positioning

The project remains about LLM package hallucinations, slopsquatting, and software supply-chain security. The main change is what the study claims to measure.

The repository will no longer claim to reproduce complete PyPI/npm security systems or to measure a full "defense catch rate." Hidden moderation, spam/malware detection, quarantine decisions, and policy enforcement that cannot be externally reproduced will be treated as out of scope or descriptive context.

The classifier proposed in the July design will be removed from the core proposal because its target label was derived from the same rules used as features and is therefore circular unless an independent outcome label becomes available.

## 3. Core Research Questions

**RQ1 — Residual exposure**  
What proportion of LLM-generated nonexistent package recommendations remain in the residual candidate set after applying publicly observable and independently reproducible package-registry name controls?

**RQ2 — Characterization**  
Which characteristics of hallucinated package recommendations are associated with remaining outside the coverage of those public controls?

**RQ3 — Stability / reproducibility**  
How stable is the residual candidate set across repeated generations, model families or versions, and registry snapshots?

**RQ4 — Taxonomy**  
How are recurring residual candidates distributed across established package-confusion categories, and which categories are least covered by the studied controls?

RQ4 is secondary rather than the primary novelty claim.

## 4. Methodological Boundaries

The study will initially focus on **PyPI and npm**. Additional ecosystems will not be added merely to inflate novelty.

Dependency extraction will use an evidence hierarchy rather than raw `import` / `require` parsing alone:

1. explicit install commands;
2. explicit dependency/package lists;
3. generated manifests;
4. reconciled imports/requires;
5. unresolved modules kept separate.

Standard-library and built-in modules, local/relative modules, aliases, scoped npm packages, and known import-to-distribution mismatches must be handled explicitly.

A package hallucination will be defined against a timestamped authoritative registry state after ecosystem-specific normalization and package/distribution reconciliation.

Every observation must preserve model identifier/version, generation timestamp, prompt/configuration, registry snapshot timestamp, normalization/extraction version, and data source/hash where practical.

## 5. Evidence Hierarchy

The literature review will distinguish three evidence classes:

1. **Peer-reviewed literature** — primary academic evidence.
2. **Concurrent/preprint literature** — used for novelty and scoop-risk assessment; not treated as equivalent to peer-reviewed evidence.
3. **Industry/operator evidence** — used for registry behavior, policies, and operational context.

The existing peer-reviewed bibliography will be preserved. Concurrent 2026 work will be tracked separately so future novelty checks cannot ignore fast-moving preprints.

## 6. Repository Changes

### Rewrite

- `README.md` — concise current project overview; no stale classifier or full-defense claims.
- `CONTEXT.md` — mark July Pivot D as superseded and document the September direction.
- `paper/proposal.md` — rewrite title, abstract, gap, RQs, methodology, metrics, threat model summary, ethics, limitations, expected contributions, pilot plan, timeline, and venue framing.
- `paper/lit_review.md` — remove "peer-reviewed only for novelty" policy and "field wide open" claims; separate evidence classes and incorporate 2026 concurrent work.
- `paper/handoff_prompt.md` — update onboarding instructions so future agents do not revive stale assumptions.

### Add

- `NOVELTY_MATRIX.md` — side-by-side comparison of closest prior/concurrent work against the revised plan.
- `THREAT_MODEL.md` — attacker, victim, attack chain, measured/unmeasured stages, assumptions, and non-claims.
- `REPRODUCIBILITY.md` — model/version logging, prompt IDs, generation settings, registry snapshots, hashes, normalization/extraction versions, seeds, environment, and release policy.
- `paper/advisor_one_pager.md` — concise advisor-facing proposal summary.
- `refs_concurrent.bib` — separate bibliography for relevant preprints/concurrent work, after metadata verification.

### Preserve

- `refs.bib` as the curated peer-reviewed bibliography unless individual entries are later found incorrect.
- current `data/`, `scripts/`, and `results/` scaffolding; no large experiments will be launched during this reorganization.

## 7. Proposal Structure

The refreshed proposal will include:

1. problem and motivation;
2. related work and explicit September 2026 gap;
3. RQs;
4. operational definitions;
5. model/prompt sampling plan;
6. dependency extraction and validation;
7. timestamped registry-state methodology;
8. public-control evaluation;
9. metrics and robustness/stability analysis;
10. threat model and non-claims;
11. ethics/responsible disclosure;
12. limitations;
13. pilot study plan;
14. six-month research timeline;
15. realistic venue framing;
16. expected contributions without unsupported "first" claims.

No numerical results will be invented. "Preliminary Results" will become **Pilot Study Plan** until real pilot data exists.

## 8. Ethics and Safety

The project will remain defensive and read-only where possible:

- no registration of hallucinated package names;
- no malicious package publication or payload deployment;
- no bypass attempts against registry controls;
- rate limits and terms respected;
- no public release of fresh high-value unclaimed recurring names before disclosure/risk review;
- aggregate or transformed data may be released when exact names create abuse risk.

## 9. Success Criteria

The repository is proposal-ready when:

- README, proposal, context, and handoff agree on the same RQs and scope;
- Churilov and other relevant 2026 work are represented in novelty assessment;
- preprints are separated from peer-reviewed evidence rather than ignored;
- the study claims only observable/reproducible registry controls;
- the classifier is removed from the core study;
- the hallucination definition and import/package mismatch are explicit;
- registry state is timestamped/snapshotted;
- threat model, non-claims, ethics, limitations, and reproducibility are explicit;
- the timeline reflects a 1–2 month proposal phase inside a 6–8 month research horizon;
- expected contributions avoid unsupported priority claims;
- an advisor-ready one-page summary exists.

## 10. Out of Scope for This Reorganization

- full-scale LLM API sweeps;
- package registration experiments;
- guessed implementations of PyPI/npm internal abuse systems;
- training Random Forest/XGBoost solely because the old proposal listed them;
- adding Go/Rust solely for novelty;
- claiming real-world exploitability from an unclaimed package name alone;
- selecting a final publication venue before pilot feasibility and advisor review.
