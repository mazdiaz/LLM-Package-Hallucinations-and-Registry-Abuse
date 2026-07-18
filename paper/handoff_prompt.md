# Handoff Prompt for New AI Sessions

Paste the block below into a fresh chat (Claude / GPT / Gemini / opencode) to onboard a new AI assistant quickly.

---

I'm working on a 2-month research proposal on "LLM Package Hallucinations and Registry Abuse" (slopsquatting). Repository: https://github.com/mazdiaz/LLM-Package-Hallucinations-and-Registry-Abuse

READ FIRST: CONTEXT.md at repo root. It has full project state, locked pivot, hard constraints, critical sources.

## Current state
- Pivot D LOCKED: "Registry Defense Benchmark" — test whether PyPI/npm anti-abuse defenses catch LLM-hallucinated package names; build evasion classifier.
- 4 RQs defined (see CONTEXT.md Section 2).
- 40 peer-reviewed sources in refs.bib, all DOI-verified, organized in 10 clusters.
- All 12 NotebookLM validation questions resolved.
- Spracklen, Neupane, Andriushchenko PDFs saved locally in paper/pdfs/.
- Zotero collection "LLM Package Hallucinations" has 40 items (key WR5TGMSC).

## HARD CONSTRAINTS — DO NOT VIOLATE
- Peer-reviewed sources ONLY. NO arXiv preprints. NO blogs, whitepapers, industry reports.
- NEVER cite Oh et al. 2026 IJAIBDCMS — flagged as predatory, removed.
- NEVER suggest using Andriushchenko 2026 Transformer as the RQ3 classifier architecture. It requires white-box LLM access we don't have. RQ3 uses standard tabular ML (logistic regression, random forest, XGBoost/LightGBM) on engineered features. (See proposal.md Section 4.6 for explicit note.)
- NEVER suggest using Levenshtein/Jaro-Winkler as primary features. Spracklen showed 48.6% of hallucinations have distance ≥6 from real packages — lexical features expected to underperform. Neupane semantic categories are primary signal.
- ALWAYS extract hallucinated names via Spracklen's 3-step procedure (LLM post-query → import cross-validation → install-command capture). NOT raw import parsing. (See proposal.md Section 4.5.1.)
- NEVER suggest registering actual packages. Ethics non-negotiable.

## What I need help with next
1. Building PyPI scraper (scripts/scrape_pypi.py) using Simple API
2. Building npm scraper (scripts/scrape_npm.py) or deps.dev API
3. Cloning Spracklen repo as reference: github.com/Spracks/PackageHallucination
4. Drafting pilot script: 2 LLMs × 100 prompts → first hallucination numbers
5. Reading PDFs (Spracklen, Neupane, Andriushchenko) and extracting methodology details

## How to help
- Verify DOIs via CrossRef before adding any new source.
- When in doubt, check CONTEXT.md "What AI Assistants Should Know" Section 9.
- Use the repo's existing structure — don't re-do work.
- Be concise. Caveman-style communication is fine for chat; normal academic register for files/commits.

Start by reading CONTEXT.md, then ask me what to work on first.
