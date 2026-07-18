# Literature Review: LLM Package Hallucinations & Slopsquatting

**Last updated:** 2026-07-18
**Status:** Merged findings (GPT-4 + direct arXiv/USENIX verification). 30 verified sources.
**Verification:** All arXiv IDs verified live 2026-07-18.

## Methodology

Sources cross-checked via:
- Direct arXiv fetch (every paper ID returns expected title)
- USENIX Security proceedings pages
- Wikipedia slopsquatting entry (citations verified)
- Vendor whitepapers (Trend Micro, Socket, CSA)
- GPT-4 lit review (every citation manually verified)

---

## Cluster 1 — Package Hallucination Measurement (CORE)

### Spracklen et al. — USENIX Security 2025 ★ SEMINAL
- **Title:** "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs"
- **arXiv:** 2406.10279 | **Code:** github.com/Spracks/PackageHallucination (MIT)
- **Affiliations:** UT San Antonio, Virginia Tech
- **Method:** 16 LLMs × 2 prompt datasets × Python+JS → 576,000 samples
- **Findings:** 19.7% avg hallucination (5.2% commercial, 21.7% open-source); 205,474 unique names; CodeLlama 7B/34B >33%
- **Mitigations:** RAG, self-detection, fine-tuning (fine-tuning = largest reduction)
- **Coverage:** RQ1 (Python/JS), RQ4 (mitigation). My RQ1 partially CLOSED.

### Churilov — arXiv 2605.17062 (May 2026)
- **Title:** "The Range Shrinks, the Threat Remains"
- **Code:** github.com/churik5/slopsquatting-replication-2026
- **Method:** 5 frontier models (Claude Sonnet 4.6, Haiku 4.5, GPT-5.4-mini, Gemini 2.5 Pro, DeepSeek V3.2) × 199,845 prompts
- **Findings:** Rates compressed to 4.62–6.10%; 127 model-agnostic hallucinated names; 53 still registrable after disclosure; DeepSeek/GPT J=0.343 (possible shared training data)
- **Coverage:** RQ1 (frontier models), RQ2 (registerability after registry defenses). My RQ2 partially CLOSED for Python/JS.

### Krishna et al. — arXiv 2501.19012 (2025)
- **Title:** "Importing Phantoms: Measuring LLM Package Hallucination Vulnerabilities"
- **Method:** Extends Spracklen to **Python, JavaScript, and Rust**
- **Findings:** 0.22%–46.15% rates depending on model/language/task; larger models hallucinate less
- **Coverage:** **Rust already covered.** My Pivot A (ecosystem expansion) severely weakened.

### Haque et al. — arXiv 2512.08213 (2025)
- **Title:** "Secure or Suspect? Investigating Package Hallucinations of Shell Command in Original and Quantized LLMs"
- **Method:** 5 Qwen sizes, **Go** shell-command package recommendations, 3 datasets
- **Findings:** Quantization materially worsens hallucination rate and vulnerability exposure
- **Coverage:** **Go already covered** (shell-command framing). Pivot A further weakened.

### Twist et al. — arXiv 2509.22202 (2025)
- **Title:** "Library Hallucinations in LLMs: Risk Analysis Grounded in Developer Queries"
- **Method:** 6 LLMs, prompt-variation stress tests (typos, temporal queries, fake library names)
- **Findings:** One-character typos → 26% hallucination; fake names accepted up to 99%; temporal prompts → up to 84%
- **Coverage:** Prompt-robustness angle of RQ1. Novel factor: realistic developer queries.

### Lanyado — Lasso Security / Vulcan (2023-2024)
- **Original case study:** huggingface-cli hallucination, 30k+ downloads in 3 months (2023)
- **Extended study (2024):** 47,803 how-to questions, 5 languages, GPT-3.5/4/Gemini/Cohere; 215 cross-model hallucinated names

### Large-Scale Skill Recommendation Study — arXiv 2607.12340 (July 2026)
- Extends hallucination analysis to "skills" — adjacent to agentic tool recommendation
- Most recent prior work; gap on **MCP/agent tools** still open

---

## Cluster 2 — Mitigation of Code/Library/API Hallucination

### Eghbali & Pradel — arXiv 2401.01701 (2024) "De-Hallucinator"
- Iterative grounding with retrieved API references for code completion / test generation
- Reduces API/library hallucinations; project-API focused, not package-registry

### Chen et al. — arXiv 2505.05057 (2025) "MARIN"
- Hierarchical dependency mining + constrained decoding
- APIHulBench; outperforms RAG on API hallucination

### Miranda-Pena et al. — arXiv 2604.07755 (2026)
- Static analysis detects 14–85% of library hallucinations
- Limited ceiling; multiple LLMs and analyzers

### Khati et al. — arXiv 2601.19106 (2026)
- Deterministic AST post-processor + library introspection
- High precision on knowledge-conflicting errors; small curated Python set

**Gap:** None directly solves "lightweight package-name classifier across registries pre-install." Real opening remains.

---

## Cluster 3 — Software Supply Chain / Malicious Package Detection

### Taylor et al. — SpellBound (arXiv 2003.03471, 2020)
- Pre-install lexical/popularity warnings for typosquatting
- 0.5% install warnings, ~2.5% overhead

### Vu et al. — EuroS&PW 2020
- PyPI typosquatting + combosquatting detection

### Duan et al. — NDSS 2021 (MALOSS)
- 339 new malicious packages discovered, 278 removed
- Multi-signal measurement framework

### Zimmermann et al. — USENIX Security 2019
- npm ecosystem: dependency graph, maintainer concentration, blast radius

### Ladisa et al. — IEEE S&P 2023 (SoK)
- 107 attack vectors, 94 incidents, 33 safeguards across OSS supply chains

### Ladisa et al. — ACSAC 2023
- Cross-language malicious-package detection (npm + PyPI); 58 new malicious packages removed

### Huang et al. — DONAPI, USENIX Security 2024
- Behavior-sequence knowledge mapping for npm malicious detection

### Guo et al. — PyGuard, USENIX Security 2026
- Knowledge-mining framework; 219 new malicious PyPI packages found

---

## Cluster 4 — Registry Policy / Operational Response (WEAKEST)

### PyPI Help / Project Quarantine (2024)
- Confusable-name rejection at registration
- Quarantine mechanism (since Aug 2024); ~140 projects quarantined
- Report-as-malware workflow

### npm Disputes Policy
- First-come, first-served
- Narrow squatting definition; name transfers generally not done on demand

### Systematic Review (ResearchGate 2026)
- 21 peer-reviewed + 7 industry reports synthesized
- Maps defenses at IDE / registry / CI-CD / runtime layers
- **READ FIRST for orientation**

### Industry
- **Trend Micro** (Sean Park, 2026) — Principal Threat Researcher whitepaper
- **Socket** — "Rise of Slopsquatting" (2025)
- **Cloud Security Alliance** — Research Note (Apr 2026)
- **VulnIQ** — original 2024 industry blog
- **Governance:** Oh et al. IJAIBDCMS 2026 — PCI-DSS CI/CD pipelines

---

## Gaps Identified (research opportunities)

After Spracklen + Churilov + Krishna + Haque + Twist + mitigation cluster:

| Gap | Status | Notes |
|-----|--------|-------|
| 1. Other ecosystems (Go, Rust, Ruby, Maven, NuGet) | **Mostly CLOSED** | Krishna=Rust, Haque=Go. Open: Ruby, Maven, NuGet, Swift, R, PHP, C++ |
| 2. Agentic / MCP tool hallucination | **OPEN** | No canonical study. arXiv:2607.12340 starts on "skills". |
| 3. Long-context / multi-turn effects | OPEN | Not studied |
| 4. Real attack telemetry | OPEN | No confirmed in-the-wild attack (Wikipedia Jul 2026) |
| 5. Adversarial fine-tuning / poisoning | OPEN | Not studied |
| 6. Cross-lingual transfer | OPEN | Not studied |
| 7. Cost-of-attack model | OPEN | Per-registry friction not quantified |
| 8. IDE-specific codegen paths | OPEN | Copilot, Cursor, Cody not directly studied |
| 9. Static-analysis detection at install | PARTIAL | Miranda-Pena, Khati touch this; registry-integrated classifier missing |
| 10. **RQ3 — Registry defense benchmark** | **WIDE OPEN** | GPT-4 + my review both confirm no peer-reviewed study measures how often PyPI/npm defenses catch hallucination-driven registrations |
| 11. **Cross-registry registerability systematic measurement** | OPEN | Churilov did post-disclosure on 53 names; broader systematic measurement missing |

---

## Revised Pivots (for 2-month proposal)

### ~~Pivot A — Ecosystem Expansion~~ [DEAD]
Krishna (2025) covers Rust, Haque (2025) covers Go. Only Ruby/Maven/NuGet/Swift remain — too thin.

### Pivot B — Agentic / MCP Hallucination [HIGH NOVELTY, RECOMMENDED]
Hallucinations in agent tool calls (MCP servers, LangChain tools, OpenAI function-calling schema). No canonical registry of valid tools → must construct ground truth.
- **Pros:** Hot topic, no direct prior work, high impact
- **Cons:** Ground truth construction is research design challenge

### Pivot C — Adversarial Fine-Tuning Attack [VERY HIGH NOVELTY]
Threat model: attacker publishes fine-tuned model or poisoned dataset that steers hallucinations toward attacker-owned package names.
- **Pros:** Novel threat model, security venue appeal
- **Cons:** Needs GPU; full study >2 months; proposal-only feasible

### Pivot D — Registry Defense Benchmark [STRONGEST GAP, RECOMMENDED]
Systematically test whether PyPI and npm anti-abuse controls stop hallucination-driven registrations. Use Churilov's 53 registrable names + new ones from replications.
- **Method:** Generate hallucinated names from current frontier models (extending Churilov); ethically measure which names would pass each registry's defenses using their published rules; build a static classifier predicting "evades defenses" vs "blocked".
- **Pros:** Fills the clearest identified gap (both reviews confirm); reproducible; defensible ethics (no registration, only rule-based simulation + minimal coordination with registry teams); 2-month feasible
- **Cons:** Less sexy than Pivot B; needs careful ethics framing

### Pivot E — Prompt-Variation Robustness Benchmark [MEDIUM NOVELTY]
Extend Twist et al. with systematic prompt perturbation across all current frontier models × all major registries.
- **Pros:** Methodology exists (Twist); reproducible
- **Cons:** Twist already partially done; may be seen as incremental

---

## Top Recommendation

**Pivot D (Registry Defense Benchmark).** Reasons:
- Clearest open gap (both independent reviews confirm)
- Builds directly on Churilov's coordinated-disclosure findings (53 names)
- Does NOT require registering packages — fully ethical / undergraduate-safe
- Produces novel dataset + classifier (RQ4-style mitigation contributes too)
- Realistic 2-month timeline for proposal + preliminary results
- Strong security venue appeal (NDSS/CCS/USENIX shape)

**Backup:** Pivot B (MCP) if you can construct a clean ground-truth tool registry within week 1.

---

## Next Actions

1. **Read Spracklen PDF in full** — download from `https://www.usenix.org/system/files/conference/usenixsecurity25/sec25cycle1-prepub-742-spracklen.pdf`
2. **Read Churilov (10 pages, fast)** — arXiv:2605.17062
3. **Read Systematic Review** — ResearchGate link, fastest orientation
4. **Pick pivot (D or B)** by end of day
5. **Update `proposal.md`** with chosen pivot's RQs
