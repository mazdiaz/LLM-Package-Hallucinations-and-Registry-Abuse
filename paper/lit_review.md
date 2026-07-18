# Literature Review: LLM Package Hallucinations & Slopsquatting

**Last updated:** 2026-07-18
**Status:** Peer-reviewed only (18 sources in `refs.bib`). arXiv preprints held in `refs_arxiv_pending.bib` pending advisor approval. Blogs/whitepapers logged in `non_citable_context.md`.

## Methodology

Sources cross-checked via:
- CrossRef DOI resolution (every DOI returns expected title)
- USENIX / IEEE / ACM proceedings pages
- arXiv ID verification (live fetch)
- Wikipedia slopsquatting entry (used as pointer only — not cited)

---

## Cluster 1 — Package Hallucination (Core)

Only **one** peer-reviewed paper directly studies package hallucination by LLMs at scale.

### Spracklen et al. — USENIX Security 2025 ★ SEMINAL
- **Title:** "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs"
- **Affiliations:** UT San Antonio, Virginia Tech
- **Method:** 16 LLMs × 2 prompt datasets × Python+JS → 576,000 samples
- **Findings:** 19.7% avg hallucination (5.2% commercial, 21.7% open-source); 205,474 unique names; CodeLlama 7B/34B >33%
- **Mitigations evaluated:** RAG, self-detection, fine-tuning (fine-tuning = largest reduction)
- **Coverage:** RQ1 (Python/JS), RQ4 (mitigation)

**Implication:** Only one peer-reviewed paper exists on this exact topic. Field is wide open for follow-up work. Replication on 2026 frontier models and other ecosystems is publishable.

---

## Cluster 2 — Software Supply Chain Attack Taxonomies & Ecosystems

### Zimmermann et al. — USENIX Security 2019
"Small World with High Risks" — npm dependency/maintainer graph; blast-radius analysis.

### Oh et al. — DIMVA 2020
"Backstabber's Knife Collection" — Review of OSS supply chain attacks; dataset of malicious packages.

### Ladisa et al. — IEEE S&P 2023 (SoK)
"Taxonomy of Attacks on Open-Source Software Supply Chains" — 107 attack vectors, 94 incidents, 33 safeguards. Industry-standard reference for situating slopsquatting.

---

## Cluster 3 — Typosquatting / Combosquatting / Source-Package Discrepancy

### Vu et al. — IEEE EuroS&PW 2020
"Typosquatting and Combosquatting Attacks on the Python Ecosystem" — Pre-LLM slopsquatting predecessor. Direct lineage.

### Vu et al. — ESEC/FSE 2021 (LastPyMile)
Identifies discrepancy between published package and its source repo. Defense primitive useful for RQ4 framing.

---

## Cluster 4 — Malicious Package Detection (Defenses)

### Duan et al. — NDSS 2021 (MALOSS)
Multi-signal framework; 339 new malicious packages discovered, 278 removed.

### Scalco et al. — ARES 2022
npm injection detection.

### Ladisa et al. — SCORED@CCS 2022
Malicious Java package detection.

### Ladisa et al. — SCORED@CCS 2023 (Hitchhiker's Guide)
Malicious third-party dependencies; multi-ecosystem.

### Ladisa et al. — ACSAC 2023
Cross-language malicious package detection (npm + PyPI); 58 new malicious packages removed.

### Huang et al. — USENIX Security 2024 (DONAPI)
Behavior-sequence knowledge mapping for npm malicious package detection.

### Liu et al. — ASE 2024
"Towards Robust Detection of OSS Supply Chain Poisoning Attacks in Industry Environments."

### SpiderScan — ASE 2024
Graph-based behavior modeling for npm malicious package detection.

### Gao et al. — USENIX Security 2025 (MalGuard)
Real-time malicious package detection; 113 new malicious packages found.

### Guo et al. — USENIX Security 2026 (PyGuard)
Knowledge-mining framework for PyPI; 219 new malicious packages found.

**Implication:** Malicious-package DETECTION is well-studied. Pre-INSTALL defense against hallucinated names specifically is NOT.

---

## Cluster 5 — Dependency Network Empirical Studies

### Zerouali et al. — Empirical Software Engineering (journal) 2022
Security vulnerability impact in npm and RubyGems dependency networks.

---

## Cluster 6 — Governance / Adjacent

### Oh et al. — IJAIBDCMS 2026
Governing AI-Generated Code in PCI-DSS Compliant CI/CD Pipelines.

---

## Gaps Identified (research opportunities)

After 18 peer-reviewed sources reviewed:

| Gap | Status | Notes |
|-----|--------|-------|
| 1. **Replication of Spracklen on 2026 frontier models** | **OPEN in peer-reviewed literature** | Churilov preprint exists but not peer-reviewed |
| 2. **Ecosystems beyond Python/JS** | **OPEN in peer-reviewed literature** | Preprints exist for Rust (Krishna) and Go (Haque) but unpublished |
| 3. **Registry defense benchmark (RQ3)** | **WIDE OPEN** | No peer-reviewed study measures whether PyPI/npm anti-abuse controls catch hallucination-driven registrations |
| 4. **Agentic / MCP tool hallucination** | **OPEN** | No peer-reviewed study |
| 5. **Long-context / multi-turn effects** | **OPEN** | Not studied |
| 6. **Adversarial fine-tuning / poisoning** | **OPEN** | Not studied |
| 7. **IDE-specific codegen paths** (Copilot, Cursor) | **OPEN** | Not studied directly |
| 8. **Cross-lingual transfer of hallucinations** | **OPEN** | Not studied |
| 9. **Real-world attack telemetry** | **OPEN** | No confirmed in-the-wild attack (per Wikipedia Jul 2026) |

---

## Revised Pivots (for 2-month proposal)

### ~~Pivot A — Ecosystem Expansion~~ [WEAKENED]
Preprints (Krishna=Rust, Haque=Go) exist; if those get peer-reviewed before yours, you're scooped. Risky.

### Pivot B — Agentic / MCP Hallucination [HIGH NOVELTY]
Hallucinations in agent tool calls (MCP servers, LangChain tools, function-calling schema). No peer-reviewed prior work.
- **Pros:** Hot topic, fully open in peer-reviewed literature
- **Cons:** Must construct ground-truth tool registry

### Pivot C — Adversarial Fine-Tuning Attack [VERY HIGH NOVELTY]
Attacker fine-tunes model or poisons data to steer hallucinations to owned names.
- **Pros:** Novel threat model
- **Cons:** Needs GPU; full study >2 months

### Pivot D — Registry Defense Benchmark [STRONGEST GAP, RECOMMENDED]
Systematically test whether PyPI/npm anti-abuse controls catch hallucination-driven registrations.
- **Method:** Use Spracklen/Churilov methodology to generate hallucinated names from current frontier models; ethically simulate each registry's defense rules; measure pass/block rates; build classifier predicting evasion.
- **Pros:** Clearest peer-reviewed gap; reproducible; no package registration needed (fully ethical); 2-mo feasible
- **Cons:** Less novel than Pivot B/C

### Pivot E — Prompt-Variation Robustness
Systematic perturbation across models × registries. Twist preprint exists but not peer-reviewed; could be first peer-reviewed.

---

## Top Recommendation

**Pivot D (Registry Defense Benchmark).** Reasons:
- Clearest peer-reviewed gap (Cluster 4 detection papers don't cover pre-install name validation against hallucinations)
- Only one peer-reviewed paper (Spracklen) exists on core topic — strong differentiation possible
- Builds on Spracklen methodology (code public, MIT licensed)
- No package registration → fully ethical
- 2-mo timeline realistic for proposal + preliminary results
- Strong security venue appeal (NDSS/CCS/USENIX shape)

**Backup:** Pivot B (MCP/agentic) if ground-truth registry can be constructed in week 1.

---

## Pending — Awaiting Advisor Decision on arXiv Policy

If advisor approves arXiv preprints, incorporate from `refs_arxiv_pending.bib`:
- Churilov 2026 (replication on frontier models)
- Krishna 2025 (Rust)
- Haque 2025 (Go, quantization)
- Twist 2025 (prompt variation)
- Eghbali 2024 De-Hallucinator, Chen 2025 MARIN, Miranda-Pena 2026, Khati 2026 (mitigation cluster)
- Skill-recommendation study (arXiv:2607.12340)

Adding these would strengthen related work and re-close some "gaps" above, further narrowing toward Pivot D as the clearest opening.

---

## Next Actions

1. **Confirm arXiv policy with advisor**
2. **Read Spracklen PDF in full** (only peer-reviewed core paper)
3. **Read Ladisa SoK (IEEE S&P 2023)** for supply-chain framing
4. **Read Vu EuroS&PW 2020** for typosquatting defense precedent
5. **Pick pivot (D or B)** by end of day
6. **Update `proposal.md`** with chosen pivot's RQs
