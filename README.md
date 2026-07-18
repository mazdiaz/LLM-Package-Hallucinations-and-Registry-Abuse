# LLM Package Hallucinations and Registry Abuse

Research investigating how often large language models recommend non-existent software packages, and the resulting supply-chain attack surface (slopsquatting).

## Problem

Code-generating LLMs frequently emit `import` statements referencing packages that do not exist in real registries. Attackers can register these hallucinated names with malicious code; users who follow model suggestions become victims. This phenomenon — "package hallucination" or "slopsquatting" — is an emerging software supply chain threat.

## Research Questions

- **RQ1**: How frequently do state-of-the-art LLMs hallucinate packages across Python, JavaScript, Go, and Rust ecosystems?
- **RQ2**: What fraction of hallucinated package names are currently registerable (unclaimed), quantifying live attack surface?
- **RQ3**: Do existing registry anti-squatting defenses (PyPI name policy, npm spam detection) catch hallucination-driven registrations?
- **RQ4**: Can a lightweight classifier on package metadata + code context flag hallucinations pre-execution?

## Methodology

1. Run N LLMs on standard code-generation benchmarks (HumanEval+, DS-1000, SWE-bench-lite).
2. Extract all package references (`import`, `require`, `use`, `#include`).
3. Resolve each against live registry snapshots (PyPI, npm, Go proxy, crates.io).
4. Classify: real / hallucinated / typo-variant.
5. Check registerability of hallucinated names (read-only — no registration).
6. Train and evaluate mitigation classifier.

## Data Sources

| Source | Purpose |
|--------|---------|
| PyPI Simple API | Python package ground truth |
| npm registry | JavaScript package ground truth |
| deps.dev API | Cross-registry metadata |
| OSV.dev | Vulnerability data |
| HumanEval+, DS-1000, SWE-bench-lite | Coding tasks |

## Status

Proposal stage — see `paper/proposal.md`.

## Ethics

- No package registration by researchers.
- Hallucinated name lists shared only with affected registries prior to publication.
- Responsible disclosure timeline followed.

## License

MIT — see [LICENSE](LICENSE).
