# Proposal-Ready Reorganization Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Apply the approved September 2026 research redesign so the repository presents a coherent, advisor-ready proposal centered on reproducible public registry controls, residual exposure, and temporal robustness.

**Architecture:** This is a documentation-first reorganization. Core research positioning is centralized in the proposal, with README and CONTEXT serving as concise summaries, while NOVELTY_MATRIX, THREAT_MODEL, and REPRODUCIBILITY isolate supporting concerns. The literature review separates peer-reviewed, concurrent/preprint, and operator/industry evidence so novelty claims can stay current without conflating evidence quality.

**Tech Stack:** Markdown, BibTeX, GitHub repository contents API.

**Spec:** `docs/superpowers/specs/2026-09-14-proposal-ready-reorganization-design.md`

## Global Constraints

- Keep the project focused on LLM package hallucinations, slopsquatting, PyPI, and npm.
- Remove the July 2026 “Registry Defense Benchmark + evasion classifier” as the locked primary novelty.
- Do not claim to reproduce hidden or internal PyPI/npm moderation, spam, malware, quarantine, or abuse systems.
- Remove the classifier from the core proposal unless an independent outcome label is later available.
- Preserve `refs.bib` as the curated peer-reviewed bibliography unless a specific entry is later invalidated.
- Track concurrent/preprint work separately from peer-reviewed evidence.
- No package registration, malicious publication, payload deployment, or bypass attempts.
- No invented results. Use “Pilot Study Plan” until real pilot data exists.
- Treat the first 1–2 months as proposal/pilot preparation within a 6–8 month research horizon.
- Keep `data/`, `scripts/`, and `results/` scaffolding; do not launch the full experiment during this change.

---

### Task 1: Rewrite the repository overview and project context

**Files:**
- Modify: `README.md`
- Modify: `CONTEXT.md`

**Interfaces:**
- Consumes: approved spec RQs, methodological boundaries, ethics, timeline.
- Produces: canonical concise project summary used by future readers and agents.

- [ ] **Step 1: Rewrite `README.md`**
  - Present the revised project title/direction.
  - Include the four revised RQs.
  - Explain “publicly observable and independently reproducible controls.”
  - Replace raw `import`/`require` extraction wording with the evidence hierarchy.
  - Remove classifier and full-defense-simulation claims.
  - Link the proposal, novelty matrix, threat model, reproducibility document, and advisor one-pager.

- [ ] **Step 2: Rewrite `CONTEXT.md`**
  - Mark the July Pivot D as superseded.
  - Record the September 2026 D2 direction.
  - Explain peer-reviewed vs concurrent/preprint vs operator evidence.
  - Record the current repo status and immediate next actions: advisor review, pilot pipeline, then experiment.
  - Explicitly instruct future agents not to revive the classifier or hidden-defense claims.

- [ ] **Step 3: Verify consistency**
  - Confirm README and CONTEXT use the same RQ wording and same PyPI/npm scope.
  - Confirm neither file contains “field wide open,” “first peer-reviewed measurement,” “actual defense catch rate,” or “evasion classifier” as a current claim.

- [ ] **Step 4: Commit**
  - Commit message: `docs: align project overview with revised proposal direction`

### Task 2: Rewrite the main proposal

**Files:**
- Modify: `paper/proposal.md`

**Interfaces:**
- Consumes: approved spec, foundational peer-reviewed literature already in the repo, September 2026 handoff findings.
- Produces: primary advisor-facing research proposal from which all shorter documents derive.

- [ ] **Step 1: Replace stale header and abstract**
  - Use a neutral working title such as “A Reproducible Study of LLM Package Hallucinations and Public Registry Controls.”
  - State target as a peer-reviewed cybersecurity/software-engineering venue to be selected with advisors after pilot feasibility.
  - Write a proposal abstract with no invented numerical results.

- [ ] **Step 2: Rewrite introduction and gap**
  - Establish package hallucinations as a known phenomenon.
  - Acknowledge recent work on frontier-model replication, registrability, mitigation, detection, and agentic workflows.
  - State the narrower gap: reproducible public-control coverage, residual candidate exposure, and temporal sensitivity.
  - Avoid priority claims.

- [ ] **Step 3: Replace RQs with the approved four RQs**
  - RQ1 residual exposure.
  - RQ2 characterization.
  - RQ3 stability/reproducibility.
  - RQ4 package-confusion taxonomy as secondary analysis.

- [ ] **Step 4: Rewrite methodology**
  - Scope PyPI and npm.
  - Define model panel selection and version/configuration logging.
  - Define prompt families: Spracklen-compatible comparability subset plus realistic dependency-seeking tasks, with adversarial tasks optional and separated.
  - Define dependency-extraction evidence hierarchy and manual validation sample.
  - Define operational package-hallucination concept against timestamped registry state.
  - Define registry snapshot metadata and public-control evaluation table.
  - Separate reproducible name-level controls from governance, post-publication controls, and undisclosed/internal controls.

- [ ] **Step 5: Define metrics and analysis**
  - Hallucination metrics: package-level, response-level, unique names, recurrence, cross-model recurrence.
  - Registry/control metrics: unclaimed rate, per-control coverage, combined coverage, residual candidate rate/count.
  - Stability metrics: Jaccard overlap, persistence, transition rates, snapshot sensitivity.
  - RQ2 uses descriptive/inferential association analysis rather than a circular supervised classifier.

- [ ] **Step 6: Add threat model, ethics, limitations, and non-claims**
  - Refer to the dedicated supporting docs.
  - State that the study does not prove malicious registration, victim installation, malware execution, or success against undisclosed controls.

- [ ] **Step 7: Replace preliminary results and timeline**
  - Rename to `Pilot Study Plan`.
  - Use a six-month core research timeline with months 7–8 available for revision if needed.
  - Explain that proposal/pilot preparation occupies the first 1–2 months.

- [ ] **Step 8: Rewrite expected contributions**
  - Measurement/reproducibility contribution.
  - Public-control coverage contribution.
  - Residual-surface characterization.
  - Temporal robustness contribution if implemented.
  - Practical defense-placement recommendations.
  - No unsupported “first” wording.

- [ ] **Step 9: Commit**
  - Commit message: `docs: rewrite proposal around reproducible public-control coverage`

### Task 3: Rebuild the literature review around evidence classes and novelty risk

**Files:**
- Modify: `paper/lit_review.md`
- Create: `refs_concurrent.bib`

**Interfaces:**
- Consumes: existing peer-reviewed bibliography and the September handoff’s identified concurrent works.
- Produces: literature foundation for the proposal and novelty matrix.

- [ ] **Step 1: Rewrite literature-review policy**
  - Section A: peer-reviewed literature as primary academic evidence.
  - Section B: concurrent/preprint literature for novelty/scoop-risk assessment.
  - Section C: industry/operator evidence for registry behavior and operational context.

- [ ] **Step 2: Preserve and regroup the useful peer-reviewed clusters**
  - Package hallucinations.
  - Package confusion / typosquatting / dependency confusion.
  - Malicious-package and install-time defenses.
  - AI code-assistant security and code hallucination.
  - Supply-chain integrity/context.
  - End each cluster with relevance to the revised RQs rather than old classifier justification.

- [ ] **Step 3: Add concurrent 2026 work that affects novelty**
  - Churilov frontier-model replication / registrability.
  - Djire et al. inference-time defenses / measurement correctness.
  - PackMonitor.
  - BOUND.
  - Names Can Hurt.
  - Bayesian-calibrated detection.
  - Registration-census work.
  - Trend Micro agentic/vibe-coding research.
  - Mark peer-review status as concurrent/preprint/industry unless independently verified otherwise.

- [ ] **Step 4: Create `refs_concurrent.bib`**
  - Include BibTeX entries only for concurrent works whose bibliographic metadata is sufficiently identified from the handoff/source pages.
  - Keep operator URLs and non-paper industry items in the literature review rather than forcing malformed BibTeX.
  - Label entries so readers can distinguish arXiv/preprint status.

- [ ] **Step 5: Replace stale gap table**
  - State that raw rate replication, generic registrability testing, generic classifier/detector work, generic mitigation, and generic agentic/MCP study are crowded.
  - Position public-control reproducibility and temporal robustness as the working differentiators that still require ongoing novelty revalidation.

- [ ] **Step 6: Commit**
  - Commit message: `docs: update literature review for 2026 novelty landscape`

### Task 4: Add the novelty matrix

**Files:**
- Create: `NOVELTY_MATRIX.md`

**Interfaces:**
- Consumes: revised literature review.
- Produces: explicit anti-duplication comparison used for advisor review and future novelty checks.

- [ ] **Step 1: Build comparison table**
  - Rows: Spracklen 2025, Neupane 2023, Churilov 2026, Djire et al. 2026, PackMonitor 2026, BOUND 2026, Names Can Hurt 2026, Bayesian-calibrated detection 2026, registration-census work, Trend Micro agentic/vibe-coding work, revised student plan.
  - Columns: status, models, ecosystems, prompt/task source, primary question, registry state, registry controls, temporal design, detector/mitigation, difference from revised plan.

- [ ] **Step 2: Add interpretation**
  - Explain which old ideas are partially scooped or crowded.
  - State the current differentiators and uncertainty.
  - Add a rule that novelty must be rechecked before final proposal/submission.

- [ ] **Step 3: Commit**
  - Commit message: `docs: add novelty matrix for proposal differentiation`

### Task 5: Add the dedicated threat model

**Files:**
- Create: `THREAT_MODEL.md`

**Interfaces:**
- Consumes: proposal scope and ethics.
- Produces: canonical attack-chain and non-claim definition referenced by proposal and README.

- [ ] **Step 1: Define actors and trust boundaries**
  - Attacker can query LLMs, identify recurring hallucinations, and potentially attempt legitimate package registration.
  - Victim is a developer or coding agent that trusts generated dependency advice.
  - Registries/package managers sit between recommendation and resolution.

- [ ] **Step 2: Define attack chain**
  - LLM emits nonexistent package → attacker identifies/predicts name → registration opportunity exists → harmful/deceptive package is published → victim/agent attempts installation → registry resolves attacker package → meaningful execution occurs.

- [ ] **Step 3: Mark measured and unmeasured stages**
  - Measured: generation behavior, registry existence at recorded snapshot, recurrence, public-control coverage, residual candidates, temporal transitions where collected.
  - Not measured/proven: malicious registration, victim installation, malware execution, internal-control bypass, compromise prevalence.

- [ ] **Step 4: Add assumptions and terminology rules**
  - Prefer “residual candidate exposure/surface.”
  - Do not call an unclaimed name “confirmed exploitable.”

- [ ] **Step 5: Commit**
  - Commit message: `docs: add explicit slopsquatting threat model and non-claims`

### Task 6: Add the reproducibility protocol

**Files:**
- Create: `REPRODUCIBILITY.md`

**Interfaces:**
- Consumes: proposal methodology.
- Produces: reproducibility requirements for later scripts/data collection.

- [ ] **Step 1: Define generation logging schema**
  - Provider, exact model ID/version, date/time, system prompt, user prompt/prompt ID, temperature, top-p, max tokens, reasoning mode if exposed, repetition index, API/library version.

- [ ] **Step 2: Define registry snapshot schema**
  - Ecosystem, acquisition time, source endpoint, checksum/hash, package-name count, normalization version, license/terms note, filtering logic.

- [ ] **Step 3: Define extraction/normalization versioning**
  - Explicit install commands, dependency lists, manifests, reconciled imports, unresolved modules.
  - Standard-library/built-in filters and mapping rules versioned.

- [ ] **Step 4: Define analysis/release policy**
  - Seeds where applicable, environment/dependency lock, data lineage, manual-validation sample, aggregate/restricted handling for security-sensitive exact names.

- [ ] **Step 5: Commit**
  - Commit message: `docs: add reproducibility protocol for model and registry snapshots`

### Task 7: Add advisor one-pager and refresh AI handoff

**Files:**
- Create: `paper/advisor_one_pager.md`
- Modify: `paper/handoff_prompt.md`

**Interfaces:**
- Consumes: final proposal, novelty matrix, threat model, reproducibility document.
- Produces: concise human and agent onboarding artifacts.

- [ ] **Step 1: Write advisor one-pager**
  - Problem and motivation.
  - Why the July idea changed.
  - Revised thesis and four RQs.
  - Method in compact form.
  - Expected contribution and feasibility.
  - Ethics/non-claims.
  - Decisions requested from advisors: approve RQs, scope, pilot, and whether to emphasize temporal design.

- [ ] **Step 2: Rewrite handoff prompt**
  - Point to `CONTEXT.md`, proposal, novelty matrix, threat model, reproducibility doc.
  - State old Pivot D is superseded.
  - Instruct future agents to include concurrent/preprint work in novelty checks.
  - Forbid reviving circular classifier, hidden-defense simulation, or unsupported priority claims without new evidence/advisor decision.

- [ ] **Step 3: Commit**
  - Commit message: `docs: add advisor summary and refresh agent handoff`

### Task 8: Cross-document verification and PR readiness

**Files:**
- Review all files changed by Tasks 1–7 plus the approved spec and implementation plan.

**Interfaces:**
- Consumes: completed documentation set.
- Produces: internally consistent proposal-ready branch and reviewable PR.

- [ ] **Step 1: Verify prohibited stale claims are absent as current claims**
  - Search/review for: `field wide open`, `first peer-reviewed`, `actual defense catch rate`, `evasion classifier`, `Pivot D locked`, `arXiv preprints EXCLUDED`, `USENIX Security / CCS / NDSS / EuroS&P shape`.
  - Historical mentions are allowed only when explicitly marked superseded.

- [ ] **Step 2: Verify required concepts are present**
  - Publicly observable/reproducible controls.
  - Residual candidate exposure.
  - Timestamped registry state.
  - Import/module-to-distribution reconciliation.
  - Threat model/non-claims.
  - Responsible disclosure.
  - 6–8 month research horizon and 1–2 month proposal/pilot phase.

- [ ] **Step 3: Compare branch to `main`**
  - Confirm only intended documentation/research-positioning files changed.
  - Confirm no experiment code/data was accidentally added.

- [ ] **Step 4: Update PR**
  - Replace WIP title/body with a concise summary of the final changes and verification performed.
  - Keep as draft unless the user explicitly asks to mark ready or merge.

- [ ] **Step 5: Final verification commit if needed**
  - Commit message: `docs: finalize proposal-ready research reorganization`
