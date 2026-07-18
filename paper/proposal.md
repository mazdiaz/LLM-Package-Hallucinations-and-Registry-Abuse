# Proposal: LLM Package Hallucinations and Registry Abuse

**Author:** mazdiaz
**Date:** TBD
**Target venue:** TBD (USENIX Security / CCS / NDSS / EuroS&P shape)
**Primary pivot:** D — Registry Defense Benchmark
**Backup pivot:** B — Agentic / MCP Hallucination (if D rejected)

## Abstract

TBD — 150 words once pilot complete.

## 1. Introduction

LLM code generators routinely emit references to software packages that do not exist in real registries — a phenomenon called *package hallucination* (Spracklen et al., USENIX Security 2025). Attackers can register these hallucinated names with malicious code, an attack pattern known as *slopsquatting* (Ladisa et al., IEEE S&P 2023). Spracklen et al. measured hallucination rates of 5.2% for commercial models and 21.7% for open-source models across 576,000 code samples in Python and JavaScript, observing 205,474 unique hallucinated package names. However, no peer-reviewed study has measured whether the anti-abuse defenses operated by major package registries (PyPI, npm) actually catch hallucination-driven registrations.

This work fills that gap by (1) systematically testing the catch rate of current registry defenses against hallucinated names, (2) identifying features that predict evasion, and (3) building a lightweight classifier that flags hallucinated names likely to evade defenses before they are registered by attackers.

## 2. Related Work

### 2.1 Package Hallucination (Core)
**Spracklen et al. (USENIX Security 2025)** — "We Have a Package for You!" Tested 16 LLMs across Python/JS, 576k samples. Found 5.2% hallucination rate for commercial models, 21.7% for open-source; 205,474 unique hallucinated names. Evaluated RAG, self-detection, fine-tuning mitigations. Only peer-reviewed paper directly on this topic.

### 2.2 Software Supply Chain Taxonomies & Ecosystems
Zimmermann et al. (USENIX Security 2019) — npm dependency graph and blast radius. Oh et al. (DIMVA 2020) — Backstabber's Knife Collection, OSS supply chain attack review. Ladisa et al. (IEEE S&P 2023) — SoK: 107 attack vectors, 94 incidents, 33 safeguards.

### 2.3 Typosquatting & Source-Package Discrepancy
Vu et al. (EuroS&PW 2020) — PyPI typosquatting and combosquatting. Vu et al. (ESEC/FSE 2021) — LastPyMile, source-package discrepancy.

### 2.4 Malicious Package Detection
Duan et al. (NDSS 2021, MALOSS) — 339 new malicious packages found. Scalco et al. (ARES 2022) — npm injection detection. Ladisa et al. (SCORED@CCS 2022, 2023; ACSAC 2023) — Java packages, third-party dependencies, cross-language detection. Sejfia & Schäfer (ICSE 2022) — practical automated detection for npm. Huang et al. (USENIX Security 2024, DONAPI) — npm behavior-sequence mapping. Liu et al. (ASE 2024) — robust detection in industry. SpiderScan (ASE 2024) — graph-based behavior modeling. Gao et al. (USENIX Security 2025, MalGuard) — real-time detection. Guo et al. (USENIX Security 2026, PyGuard) — PyPI knowledge-mining.

### 2.5 Package Confusion & Install-Time Defenses (critical for this proposal)
**Neupane et al. (USENIX Security 2023)** — "Beyond Typosquatting": 13 package-confusion categories, 360,333 confusion instances in npm. Conceptual bridge to slopsquatting. Taylor et al. (NSS 2020) — defending against package typosquatting. Ferreira et al. (ICSE 2021) — npm lightweight permission system. Wyss et al. (AsiaCCS 2022) — Latch, install-time attack mediation. Wyss et al. (ICSE 2022) — hidden code clones in npm.

### 2.6 AI Code Assistant Security
Pearce et al. (IEEE S&P 2022) — Copilot emits insecure code. Sandoval et al. (USENIX Security 2023) — user study, AI assistant security. Perry et al. (CCS 2023) — users write more insecure code with AI. Tony et al. (TOSEM 2025) — prompting for secure code. Fu et al. (TOSEM 2025) — Copilot weaknesses in GitHub. Mastropaolo et al. (ICSE 2023) — Copilot prompt robustness.

### 2.7 Code Hallucination Detection
**Andriushchenko et al. (ACL Findings 2026)** — lightweight Transformer detector for code hallucinations using internal LLM representations. Important related-work context for the broader field. **Not methodologically transferable to this proposal** (see Section 4.6 note): requires white-box LLM access unavailable for commercial APIs, and predicts code correctness rather than registry-policy evasion. Builds on **Shelmanov et al. (EMNLP 2025)** pre-trained uncertainty quantification heads. **Tian et al. (AAAI 2025)** "CodeHalu" provides execution-based verification benchmark for code hallucinations.

### 2.8 Software Composition Analysis & SBOM
Dann et al. (TSE 2022) — OSS vulnerability scanner challenges. Zhao et al. (ESEC/FSE 2023) — SCA for Java. Alfadel et al. (TOSEM 2023) — npm discoverability. Imtiaz et al. (TSE 2023) — security releases. Zahan et al. (ICSE-SEIP 2022) — weak links in npm supply chain.

### 2.9 SBOM, CI/CD, Supply Chain Integrity
Xia et al. (ICSE 2023) — SBOM empirical study. Koishybayev et al. (USENIX Security 2022) — GitHub CI workflow security. Torres-Arias et al. (USENIX Security 2019) — in-toto supply chain integrity.

See `lit_review.md` for full annotated review with 40 peer-reviewed sources.

## 3. Research Questions

- **RQ1:** How frequently do current PyPI and npm anti-abuse defenses (confusable-name rejection, quarantine, spam detection, squatting policy) catch hallucinated package names generated by state-of-the-art LLMs?
- **RQ2:** What features of a hallucinated package name predict evasion of registry defenses (e.g., lexical similarity to popular packages, model consensus, length, popularity asymmetry)?
- **RQ3:** Can a lightweight classifier predict defense evasion with sufficient precision to serve as a pre-registration early-warning tool for registry operators?
- **RQ4:** How do hallucinated names distribute across the package-confusion taxonomy of Neupane et al. (2023), and which confusion categories correlate with defense evasion?

## 4. Methodology

### 4.1 Hallucination Generation
- Run N frontier LLMs (e.g., Claude Sonnet, GPT-5, Gemini, DeepSeek, Llama, Mistral) on standard code-generation prompt datasets (Stack Overflow-derived, LLM-generated, similar to Spracklen).
- Languages: Python and JavaScript (aligns with Spracklen for direct comparability).
- Reuse Spracklen's open-source MIT-licensed pipeline (github.com/Spracks/PackageHallucination) where possible.

### 4.2 Registry Snapshot
- PyPI: full package name list via Simple API (`https://pypi.org/simple/`).
- npm: full package name list via replicate endpoint or deps.dev API.
- Snapshot date recorded for reproducibility.

### 4.3 Registerability Classification (READ-ONLY)
- For each hallucinated name: query whether the name is currently **unclaimed** on each registry.
- **No package registration performed.** Pure read-only check.

### 4.4 Defense Rule Simulation
For each hallucinated name, apply each registry's published anti-abuse rules:

**PyPI:**
- Confusable-name rejection policy (names too similar to existing projects).
- Quarantine criteria (per Mike Fiedler's Project Quarantine operational rules).
- Reserved/prohibited name lists.

**npm:**
- First-come-first-served policy.
- Narrow squatting definition (no genuine function).
- Spam-detection heuristics.

Predict: **blocked** / **passes**.

### 4.5 Feature Engineering
For each hallucinated name, extract:

- Lexical: length, character n-grams, entropy. *(Note: Spracklen et al. 2025 found hallucinated names are "substantively different from existing package names" — only 13.4% have Levenshtein distance ≤2 to real packages, while 48.6% have distance ≥6. Pure string-distance features are expected to underperform and will be included only as baseline; the classifier will lean on semantic and model-consensus features instead.)*
- Similarity: Levenshtein distance to top-N popular packages; Jaro-Winkler to top-1000 *(baseline features; expected weak signal per Spracklen)*.
- **Semantic confusion category**: Neupane et al. (2023) 13-category taxonomy assignment *(primary signal — captures "substantively different" patterns better than lexical distance)*.
- Model consensus: number of distinct LLMs that hallucinate the name identically.
- Popularity asymmetry: ratio of popularity to nearest real package.
- Code context: where the hallucinated name appeared *(per Spracklen's caveat below, extracted from the LLM's stated package list rather than from raw `import` statements, since module namespace ≠ package namespace)*.
- Registry context: nearest real package age, maintainer count, download volume (via deps.dev or libraries.io).

### 4.5.1 Hallucinated Name Extraction (per Spracklen caveat)

Spracklen et al. (2025) explicitly warn that *"simply parsing the code for 'import' or 'require' is not useful, as the arguments in those statements refer to modules and not packages."* Module namespace is not protected and lacks 1:1 mapping to package names. Following Spracklen's methodology:

1. **Primary extraction:** query the LLM post-generation for its stated package dependencies (e.g., "What packages are required to run this code?").
2. **Cross-validation:** map stated packages to actual `import`/`require` statements in the generated code where possible; flag mismatches as a separate feature.
3. **Install-command extraction:** capture any explicit `pip install` or `npm install` commands in the generated output as a tertiary source.

This three-step procedure resolves the module/package discrepancy and produces a clean hallucinated-name dataset suitable for downstream feature engineering.

### 4.6 Classifier Training
- Task: predict whether a hallucinated name will evade registry defenses.
- Models: **tabular classifiers only** — logistic regression ( interpretable baseline), random forest (interpretable non-linear baseline), gradient-boosted trees (XGBoost/LightGBM, strongest tabular baseline). All operate on the engineered feature set in Section 4.5.
- **Note on Andriushchenko 2026:** While Andriushchenko's Transformer-based detector is excellent related-work context for code hallucination detection (Cluster 8), its methodology is **not transferable to this proposal** because (a) it requires white-box access to LLM internal representations (hidden states, attention weights) which is unavailable for the commercial APIs used here, and (b) it predicts code-generation correctness, not registry-policy evasion. The classifier component of this proposal operates entirely on external tabular features (lexical metrics, popularity, taxonomy) and therefore uses standard tabular ML rather than internal-representation Transformers.
- Train/test split: 70/30 stratified by registry.
- Metrics: precision, recall, F1, ROC-AUC; per-registry breakdown.
- Ablation: drop each feature group to identify signal sources. *(Hypothesis: lexical-distance group will be weakest per Spracklen's finding; semantic-confusion and model-consensus groups will dominate.)*
- Interpretability: SHAP values on best-performing model to surface which features drive evasion predictions (actionable for registry operators).

### 4.7 Confusion Taxonomy Mapping
Cross-reference each hallucinated name to Neupane et al. (2023) 13 package-confusion categories. Quantify which categories correlate with defense evasion.

### 4.8 Ethics
- Zero package registration by researchers.
- Read-only registry queries only.
- Coordinated disclosure to PyPI Security and Socket teams if actively exploitable patterns discovered, following Spracklen/Churilov precedent.
- No release of hallucinated-name lists that could enable attacks; share only with verified researchers and registry operators.
- IRB review if required by institution.

## 5. Preliminary Results

TBD — pilot.

## 6. Timeline

| Week | Task |
|------|------|
| 1 | Methodology lock; clone Spracklen repo; PyPI/npm scrapers |
| 2 | Run 5 frontier LLMs; extract hallucinations; deduplicate |
| 3 | Apply each registry's defense rules; measure catch rates |
| 4 | Build feature set; train classifier v1 |
| 5 | Validate classifier; ablation studies |
| 6 | Cross-reference Neupane taxonomy; analyze failure cases |
| 7 | Write proposal sections + preliminary figures |
| 8 | Polish; final proposal draft |

## 7. Expected Contributions

1. **Empirical:** First peer-reviewed measurement of PyPI/npm defense catch rates against hallucination-driven registrations.
2. **Methodological:** Reproducible pipeline for testing registry defenses against LLM-generated package names.
3. **Practical:** Lightweight tabular classifier (logistic regression / random forest / gradient-boosted trees) that registry operators can use as a pre-registration early-warning signal.
4. **Taxonomic:** Mapping of hallucinated names to Neupane's package-confusion categories, showing which attack patterns dominate.

### 7.1 Novelty Statement

Unlike RQ3 and RQ4, which build on established methodological precedents (Andriushchenko 2026; Neupane et al. 2023), **RQ1 and RQ2 address a gap in the peer-reviewed literature**: no prior work has measured the catch rate of registry anti-abuse defenses against LLM-hallucinated package names. Spracklen et al. (2025) measured hallucination *rate*; this work measures defense *effectiveness* — a distinct research question. This gap is the primary contribution of this work and differentiates it from prior literature.

## 8. Ethics

See `README.md` and Section 4.8.

## 9. Backup Pivot (if D rejected)

**Pivot B — Agentic / MCP Hallucination.** Study hallucinations in agent tool calls (MCP servers, LangChain tools, OpenAI function-calling schema). No peer-reviewed study exists. Higher novelty, higher risk (ground-truth construction required). Methodology to be developed if D is not feasible.

## 10. References

See `refs.bib` (40 peer-reviewed sources).
