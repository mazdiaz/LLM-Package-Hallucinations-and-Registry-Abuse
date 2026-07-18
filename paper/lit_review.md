# Literature Review: LLM Package Hallucinations & Slopsquatting

**Last updated:** 2026-07-18
**Status:** Preliminary — verified via arXiv, USENIX, Wikipedia, vendor sources. GPT-4 cross-check in progress.

## Seminal Papers (verified)

### 1. Spracklen et al. — "We Have a Package for You!" (USENIX Security 2025)
- **Authors:** Joseph Spracklen, Raveen Wijewickrama, A.H.M. Nazmus Sakib, Anindya Maiti, Bimal Viswanath, Murtuza Jadliwala
- **Affiliations:** University of Texas at San Antonio, Virginia Tech
- **arXiv:** 2406.10279 (v1 Jun 2024, v3 Mar 2025)
- **Code:** github.com/Spracks/PackageHallucination (MIT license)
- **DOI:** 10.5281/zenodo.14676377

**Method:** 16 LLMs (commercial + open-source) × 2 prompt datasets (Stack Overflow-derived + LLM-generated) × 2 languages (Python, JavaScript). 576,000 code samples.

**Findings:**
- 19.7% of recommended packages hallucinated (overall average)
- 5.2% hallucination rate — commercial models
- 21.7% hallucination rate — open-source models
- CodeLlama 7B/34B: >33% hallucination rate (worst)
- 205,474 unique hallucinated package names observed

**Mitigations evaluated:**
- RAG (retrieval-augmented generation) — reduces rate
- Self-detection (model re-checks own output)
- Fine-tuning — **largest reduction**

**Ethics:** Master hallucinated name list NOT released; available to verified researchers.

**Covers:** RQ1 (Python/JS), RQ4 (mitigation). My RQ1 for Python/JS is largely DONE.

---

### 2. Churilov — "The Range Shrinks, the Threat Remains" (May 2026)
- **arXiv:** 2605.17062 (v1 May 2026, v2 Jun 2026)
- **Author:** Aleksandr Churilov (independent)
- **Code:** github.com/churik5/slopsquatting-replication-2026

**Method:** Replicates Spracklen methodology on 5 frontier models released Oct 2025–Mar 2026: Claude Sonnet 4.6, Claude Haiku 4.5, GPT-5.4-mini, Gemini 2.5 Pro, DeepSeek V3.2. 199,845 paired Python/JS prompts.

**Findings:**
- Hallucination rates compressed to 4.62% (Claude Haiku 4.5) — 6.10% (GPT-5.4-mini)
- Inter-model spread shrunk by order of magnitude vs Spracklen
- **127 package names all 5 models hallucinate identically** (model-agnostic)
- After coordinated disclosure to PyPI/Socket, 53 names still registrable by attacker (41 PyPI, 12 npm)
- Python-over-JS asymmetry inverts Spracklen's 2024 finding
- DeepSeek V3.2 / GPT-5.4-mini Jaccard similarity J=0.343 (possible shared training data)

**Covers:** RQ1 (2026 frontier models), RQ2 (registerability after registry defenses). RQ1 for frontier models DONE.

---

### 3. Lanyado — huggingface-cli case study (June 2023)
- **URL:** vulcan.io/blog/ai-hallucinations-package-risk
- **Finding:** ChatGPT hallucinated `huggingface-cli` package. Author registered empty package. 30,000+ downloads in 3 months. First public demonstration of attack feasibility.

### 4. Lasso Security research (May 2025)
- Extended Lanyado case study, broader analysis.

### 5. Large-Scale Skill Recommendation Hallucination Study (July 2026)
- **arXiv:** 2607.12340
- Extends to "skills" — adjacent to agentic/MCP tool hallucination.

---

## Industry / Whitepapers (verified)

- **Trend Micro** — "Slopsquatting: Hallucination in AI-Generated Code" (Sean Park, Principal Threat Researcher, 2026)
- **Socket** — "The Rise of Slopsquatting" (2025) — popular industry writeup
- **Cloud Security Alliance** — Research Note (Apr 2026)
- **VulnIQ** — original 2024 industry blog
- **BleepingComputer** / **The Register** / **SecurityWeek** / **Help Net Security** — general coverage

---

## Governance / Adjacent

- **Oh et al.** — "Governing AI-Generated Code in PCI-DSS Compliant CI/CD Pipelines" (IJAIBDCMS 2026)
- **MDPI Electronics** — "Evaluating LLMs in Cybersecurity: A Systematic Review" (2026)

---

## Systematic Review (ResearchGate, 2026)
"AI-Induced Supply-Chain Compromise: A Systematic Review of Package Hallucinations and Slopsquatting Attacks"
- 21 peer-reviewed papers + 7 industry reports
- Synthesizes: definitions, threat models, observed incidents, prevalence (5.2% / 21.7%), defense layers (IDE / registry / CI-CD / runtime), taxonomy vs typosquatting/dependency confusion.
- **READ THIS FIRST** — fastest way to know what's done.

---

## Gaps Identified (research opportunities for THIS proposal)

After Spracklen + Churilov, the following are under-explored:

### Gap 1 — Other ecosystems
Spracklen/Churilov cover only Python & JavaScript. **Open:** Go, Rust, Ruby (gems), Java (Maven), NuGet, Swift, R (CRAN), PHP (Packagist), C++ (vcpkg/Conan).

### Gap 2 — Agentic / tool-call hallucination (MCP)
Models now recommend MCP servers, agent tools, plugin names, API endpoints. arXiv:2607.12340 starts on "skills" but no comprehensive study on **agent tool registries** (MCP, LangChain tools, AutoGen).

### Gap 3 — Long-context / multi-turn effects
Does context length, conversation history, or multi-turn agentic dialogue change hallucination rate? Unknown.

### Gap 4 — Real attack telemetry
Per Wikipedia (Jul 2026): no confirmed slopsquatting attack in the wild. Longitudinal monitoring of new package registrations for known hallucinated patterns is open.

### Gap 5 — Adversarial fine-tuning / poisoning
Can attacker fine-tune model or poison training data to steer hallucinations toward attacker-owned package names? Not studied.

### Gap 6 — Cross-lingual transfer
Does fine-tuning on Python packages affect hallucination rate for JS? Open.

### Gap 7 — Cost-of-attack model
Per-registry cost/benefit analysis: registration friction, 2FA requirements, name squatting policy, attack ROI.

### Gap 8 — IDE-specific
Copilot, Cursor, Cody-specific codegen paths differ. Replication through IDE plugins not done.

### Gap 9 — Static-analysis detection at install time
Pre-install hook that flags suspicious package names using model context. Some commercial tools (Socket) claim this; no academic eval.

---

## Recommended Pivots (for 2-month proposal)

Pick ONE:

### Pivot A — Ecosystem Expansion (LOW RISK, FAST)
Apply Spracklen method to Go + Rust + Ruby + Maven. Replication + extension paper.
- Pro: Clear methodology, reuses Spracklen code, 8-week timeline realistic.
- Con: Incremental; reviewers may say "obvious extension."

### Pivot B — Agentic / MCP Hallucination (HIGH NOVELTY)
Study hallucinations in agent tool calls (MCP servers, LangChain tools, OpenAI function-calling schema).
- Pro: Hot topic, no direct prior work, high impact for agent ecosystem.
- Con: Harder to define ground truth (no canonical "registry" of valid tools).

### Pivot C — Adversarial Fine-Tuning Attack (HIGH NOVELTY)
Threat model: attacker publishes a fine-tuned model (or poisoned dataset) that steers hallucinations to attacker-owned names. Demonstrate attack, propose detection.
- Pro: Novel threat model, security venue appeal.
- Con: Requires fine-tuning infrastructure (GPU), longer than 2 months for full study but feasible for proposal.

### Pivot D — Longitudinal Registry Monitoring (MEDIUM NOVELTY)
Track new package registrations across PyPI/npm/crates.io for patterns matching known hallucinated names. Quantify real-world attack surface.
- Pro: Empirical, novel data, fills Gap 4.
- Con: IRB/ethics review may slow down; needs careful disclosure to registries.

---

## Next Actions

1. Read Spracklen paper in full (PDF in `paper/spracklen2025.pdf` — to download)
2. Read systematic review (ResearchGate)
3. Read Churilov replication paper (10 pages, fast)
4. Pick a pivot (A/B/C/D) and update `proposal.md` accordingly
5. Verify GPT's lit review output against this verified list
