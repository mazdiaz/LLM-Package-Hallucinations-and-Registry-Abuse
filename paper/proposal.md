# Proposal: LLM Package Hallucinations and Registry Abuse

**Author:** mazdiaz
**Date:** TBD
**Target venue:** TBD

## Abstract

TBD — 150 words once pilot complete.

## 1. Introduction

- Software supply chain attacks context
- Rise of code-generating LLMs in developer workflows
- Package hallucination phenomenon
- Contributions of this work

## 2. Related Work

**Seminal — Spracklen et al. (USENIX Security 2025)** "We Have a Package for You!" Tested 16 LLMs across Python/JS, 576k samples. Found 5.2% hallucination rate for commercial models, 21.7% for open-source; 205,474 unique hallucinated names. Evaluated RAG, self-detection, fine-tuning mitigations. arXiv:2406.10279.

**Replication — Churilov (2026)** arXiv:2605.17062. Re-tested 5 frontier models (Claude Sonnet 4.6, GPT-5.4-mini, Gemini 2.5 Pro, DeepSeek V3.2). Rates compressed to 4.62–6.10%. Found 127 model-agnostic hallucinated names; 53 still registrable after coordinated disclosure.

**Origin — Lanyado (2023)** vulcan.io. Demonstrated attack feasibility with `huggingface-cli` (30k+ downloads of empty package in 3 months).

**Systematic Review (2026)** Synthesis of 21 peer-reviewed + 7 industry reports. Maps defenses at IDE/registry/CI-CD/runtime layers.

**Industry:** Socket, Trend Micro (Sean Park), Cloud Security Alliance, VulnIQ.

See `lit_review.md` for full table + gaps.

**Implication:** RQ1 (Python/JS prevalence) and RQ2 (registerability) are largely ANSWERED by Spracklen + Churilov. This proposal must PIVOT to one of the identified gaps.

## 3. Research Questions (PIVOTED — TBD)

Pick ONE pivot from `lit_review.md`:

- **Pivot A — Ecosystem Expansion:** Apply Spracklen method to Go, Rust, Ruby, Maven, NuGet.
- **Pivot B — Agentic / MCP Hallucination:** Study hallucinations in agent tool calls (MCP servers, LangChain tools, function-calling).
- **Pivot C — Adversarial Fine-Tuning Attack:** Demonstrate attacker fine-tunes model to steer hallucinations to owned names.
- **Pivot D — Longitudinal Registry Monitoring:** Track real-world slopsquatting registration attempts.

**Decision needed:** [TBD by end of week 1]

## 4. Methodology

See README.md.

## 5. Preliminary Results

TBD — pilot.

## 6. Timeline

| Week | Task |
|------|------|
| 1 | Repo, scrapers, lit review |
| 2-3 | Pilot: 5 models x 500 tasks |
| 4 | Registerability analysis |
| 5-6 | Mitigation classifier v1 |
| 7 | Full results + figures |
| 8 | Proposal draft complete |

## 7. Ethics

See README.md.

## References

See `refs.bib`.
