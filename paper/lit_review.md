# Literature Review: LLM Package Hallucinations & Slopsquatting

**Last updated:** 2026-07-18
**Status:** Peer-reviewed only (40 sources in `refs.bib`). arXiv preprints EXCLUDED for this proposal cycle (cannot confirm policy with advisor; using strict academic-only bar). Blogs/whitepapers logged in `non_citable_context.md` for background only.

## Methodology

Sources cross-checked via:
- CrossRef DOI resolution (every DOI returns expected title)
- USENIX / IEEE / ACM / Springer / ACL proceedings pages
- arXiv ID verification (held separately, not cited here)

---

## Cluster 1 — Package Hallucination (Core)

**Only ONE peer-reviewed paper directly studies LLM package hallucination.**

### Spracklen et al. — USENIX Security 2025 ★ SEMINAL
- **Title:** "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs"
- **Affiliations:** UT San Antonio, Virginia Tech
- **Method:** 16 LLMs × 2 prompt datasets × Python+JS → 576,000 samples
- **Findings:** 19.7% avg hallucination (5.2% commercial, 21.7% open-source); 205,474 unique names
- **Mitigations:** RAG, self-detection, fine-tuning (fine-tuning = largest reduction)

**Implication:** Only ONE peer-reviewed paper on this exact topic. Field wide open.

---

## Cluster 2 — Supply Chain Attack Taxonomies & Ecosystems

- **Zimmermann et al. — USENIX Security 2019.** npm dependency graph, blast radius.
- **Oh et al. — DIMVA 2020.** "Backstabber's Knife Collection." Review of OSS supply chain attacks.
- **Ladisa et al. — IEEE S&P 2023.** SoK: 107 attack vectors, 94 incidents, 33 safeguards.

---

## Cluster 3 — Typosquatting & Source-Package Discrepancy

- **Vu et al. — EuroS&PW 2020.** PyPI typosquatting/combosquatting.
- **Vu et al. — ESEC/FSE 2021.** LastPyMile: source-package discrepancy.

---

## Cluster 4 — Malicious Package Detection (Defenses)

- **Duan et al. — NDSS 2021 (MALOSS).** 339 malicious packages, 278 removed.
- **Scalco et al. — ARES 2022.** npm injection detection.
- **Ladisa et al. — SCORED@CCS 2022.** Malicious Java packages.
- **Ladisa et al. — SCORED@CCS 2023.** Hitchhiker's Guide: malicious third-party dependencies.
- **Ladisa et al. — ACSAC 2023.** Cross-language detection (npm + PyPI).
- **Huang et al. — USENIX Security 2024 (DONAPI).** npm behavior-sequence mapping.
- **Liu et al. — ASE 2024.** Robust detection in industry environments.
- **SpiderScan — ASE 2024.** Graph-based behavior modeling.
- **Gao et al. — USENIX Security 2025 (MalGuard).** Real-time detection; 113 new malicious.
- **Guo et al. — USENIX Security 2026 (PyGuard).** PyPI knowledge-mining; 219 new malicious.

---

## Cluster 5 — Dependency Network Empirical Studies

- **Zerouali et al. — EMSE 2022.** Security vuln impact in npm/RubyGems dependency networks.

---

## Cluster 6 — AI Code Assistant Security ★ NEW

Establishes that LLM code assistants emit insecure code; methodology transferable to package hallucination.

- **Pearce et al. — IEEE S&P 2022.** "Asleep at the Keyboard." Copilot emits insecure code under security-relevant prompts. Methodology directly applicable: replace "insecure code" with "hallucinated package".
- **Sandoval et al. — USENIX Security 2023.** "Lost at C." User study: AI-assisted users introduced security bugs at similar rate to control; human-in-the-loop acceptance matters.
- **Perry et al. — CCS 2023.** "Do Users Write More Insecure Code with AI Assistants?" User study confirming AI assistants increase security failures.
- **Tony et al. — TOSEM 2025.** Systematic prompting techniques for secure code generation. RQ4 mitigation angle.
- **Fu et al. — TOSEM 2025.** Empirical study of Copilot code in real GitHub projects. Ecological validity.
- **Mastropaolo et al. — ICSE 2023.** Robustness of Copilot under prompt perturbations.

---

## Cluster 7 — Code Hallucination Detection (peer-reviewed) ★ NEW

- **Andriushchenko et al. — ACL Findings 2026.** ★ DIRECT RQ4 RELEVANCE.
  - "Efficient Hallucination Detection in Automatic Code Generation"
  - Trains lightweight Transformer detector on LLM internal representations
  - Releases first line-level code-hallucination dataset (Collu-Bench derived)
  - Code: github.com/datapaf/CodeHallucinationDetection
  - Closest peer-reviewed precedent for hallucination classifier (RQ4)

---

## Cluster 8 — Package Confusion & Install-Time Defenses ★ NEW (critical for Pivot D)

- **Neupane et al. — USENIX Security 2023.** ★ DIRECT BRIDGE TO SLOPSQUATTING.
  - "Beyond Typosquatting: An In-depth Look at Package Confusion"
  - 13 package-confusion categories from 1,232 historical attacks
  - 360,333 potential confusion instances in npm
  - Broadens threat model beyond typosquatting — slopsquatting fits this exact space
- **Taylor et al. — NSS 2020 (Springer LNCS).** Defending against package typosquatting. Pre-install warnings baseline.
- **Ferreira et al. — ICSE 2021.** Lightweight permission system for npm; covers 31.9% of packages at ~1% overhead.
- **Wyss et al. — AsiaCCS 2022.** "Wolf at the Door." Latch: install-time attack mediation.
- **Wyss et al. — ICSE 2022.** "What the Fork?" Hidden code clones in npm.

---

## Cluster 9 — Software Composition Analysis & Empirical Studies ★ NEW

- **Dann et al. — TSE 2022.** OSS vulnerability scanner challenges; 616-case test suite.
- **Zhao et al. — ESEC/FSE 2023.** SCA for Java vulnerability detection.
- **Alfadel et al. — TOSEM 2023.** npm vulnerability discoverability in Node.js.
- **Imtiaz et al. — TSE 2023.** Security releases of OSS packages; temporal exposure windows.

---

## Cluster 10 — SBOM, CI/CD, Supply Chain Integrity ★ NEW

- **Xia et al. — ICSE 2023.** SBOM empirical study; adoption barriers.
- **Koishybayev et al. — USENIX Security 2022.** GitHub CI workflow security; 18% repos use vulnerable actions.
- **Torres-Arias et al. — USENIX Security 2019.** in-toto: cryptographic supply-chain metadata; would prevent 83-100% of historical attacks.

---

## Gaps Identified (revised after 40 peer-reviewed sources)

| Gap | Status | Notes |
|-----|--------|-------|
| 1. Replication of Spracklen on 2026 frontier models | OPEN in peer-reviewed literature | Churilov preprint exists, unpublished |
| 2. Ecosystems beyond Python/JS | OPEN in peer-reviewed literature | Krishna (Rust) and Haque (Go) are preprints |
| 3. **Registry defense benchmark (RQ3)** | **WIDE OPEN** | No peer-reviewed study measures PyPI/npm defense catch rates against hallucination-driven names |
| 4. Agentic / MCP tool hallucination | **OPEN** | No peer-reviewed study |
| 5. Long-context / multi-turn effects | OPEN | Not studied |
| 6. Adversarial fine-tuning / poisoning | OPEN | Not studied |
| 7. IDE-specific codegen paths | OPEN | Pearce/Sandoval/Perry study Copilot generally, not hallucination |
| 8. Cross-lingual transfer of hallucinations | OPEN | Not studied |
| 9. Real-world attack telemetry | OPEN | No confirmed in-the-wild attack (Wikipedia Jul 2026) |
| 10. **Lightweight package-name hallucination classifier** | PARTIAL | Andriushchenko (2026) does general code hallucination detection; package-specific version missing |

---

## Revised Pivots (for 2-month proposal)

### ~~Pivot A — Ecosystem Expansion~~ [WEAKENED]
Krishna (Rust) + Haque (Go) preprints exist; scoop risk.

### Pivot B — Agentic / MCP Hallucination [HIGH NOVELTY]
- **Pros:** No peer-reviewed prior work
- **Cons:** Ground-truth construction required

### Pivot C — Adversarial Fine-Tuning Attack [VERY HIGH NOVELTY]
- **Pros:** Novel threat model
- **Cons:** GPU-bound; full study >2 months

### Pivot D — Registry Defense Benchmark [STRONGEST GAP, RECOMMENDED]
**Method:**
1. Replicate Spracklen prompt setup on 5-10 current frontier models (Claude Sonnet 4.x, GPT-5.x, Gemini 2.5, DeepSeek V3, Llama 4, Mistral) across Python/JS
2. Extract hallucinated package names
3. Check registerability (read-only — no actual registration)
4. **For each hallucinated name:** apply each registry's anti-abuse policy (PyPI confusable-name rejection, quarantine criteria; npm first-come rules) → predict pass/block
5. Cross-reference Neupane (2023) package-confusion taxonomy to classify hallucinated-name categories
6. Train classifier predicting "evades defenses" using features: name length, lexical similarity to popular packages, popularity asymmetry, model-consensus rate
7. Validate classifier on held-out hallucinated names

**Key support from new literature:**
- **Neupane 2023** provides package-confusion taxonomy to situate slopsquatting within
- **Andriushchenko 2026** provides methodology for hallucination detector
- **Taylor 2020** provides baseline anti-typosquatting defense formulation
- **Ferreira 2021 / Wyss 2022** provide defense layers to compare against
- **Koishybayev 2022** motivates CI/CD integration angle

**Pros:** Clearest peer-reviewed gap; strong methodology scaffolding from existing work; 2-mo feasible
**Cons:** Less novel than Pivot B/C

### Pivot E — Prompt-Variation Robustness
Mastropaolo (2023) and Tony (2025) touch this; less novel than thought.

---

## Top Recommendation: **Pivot D**

Reasons unchanged but strengthened:
- 40 peer-reviewed sources now confirm RQ3 is wide open
- Neupane (2023) gives the conceptual bridge (package confusion taxonomy)
- Andriushchenko (2026) gives methodology precedent for the classifier
- Strong differentiation from Spracklen/Churilov

---

## Next Actions

1. **Read Spracklen PDF in full** (Cluster 1 seminal)
2. **Read Neupane 2023 PDF** (Cluster 9 — direct conceptual bridge)
3. **Read Andriushchenko 2026 PDF** (Cluster 8 — RQ4 methodology)
4. **Lock pivot** (D recommended)
5. **Update `proposal.md`** with chosen pivot's RQs + methodology sketch
