# Reproducibility Protocol

**Last updated:** 2026-09-14

This document defines the minimum metadata and artifact requirements for the proposed study. The goal is to make every reported package-hallucination observation traceable to a model configuration, prompt, extraction version, and registry observation.

## 1. Reproducibility Principles

1. **Freeze what can be frozen.** Record exact model IDs, prompt IDs, processing versions, and registry observations.
2. **Timestamp what can change.** Proprietary models and public registries change over time.
3. **Preserve provenance.** Each extracted dependency should retain the evidence source that produced it.
4. **Separate raw, normalized, and analyzed data.** Do not overwrite intermediate states.
5. **Version the pipeline.** Extraction and normalization changes can alter the measured hallucination set.
6. **Document restricted artifacts.** If exact names are withheld for responsible-release reasons, state what is withheld and provide reproducible aggregate/transformed alternatives where possible.

## 2. Model-Generation Record

Each generation should record at least:

| Field | Requirement |
|---|---|
| `run_id` | unique identifier |
| `generation_timestamp` | timezone-aware timestamp |
| `provider` | provider or local runtime |
| `model_id` | exact exposed model identifier |
| `model_version` | snapshot/version if separately exposed |
| `model_family` | normalized analysis label |
| `system_prompt` | exact text or immutable reference |
| `prompt_id` | stable task identifier |
| `user_prompt` | exact text or immutable reference |
| `temperature` | value used |
| `top_p` | value used, if exposed |
| `max_tokens` | configured limit |
| `reasoning_mode` | value if exposed, otherwise `not_exposed` |
| `repetition_index` | repeated-run number for the same condition |
| `seed` | value if supported; otherwise `not_supported` |
| `client_library` | package/client name |
| `client_version` | exact version |
| `raw_output_path` | immutable artifact reference |

For local/open-weight models also record runtime, quantization if any, model-file/revision identifier, and relevant hardware/runtime version.

## 3. Prompt and Task Provenance

Each prompt/task should have a stable `prompt_id` and metadata describing:

- prompt family (`comparability`, `realistic_dependency`, optional `stress`);
- source/origin;
- language/ecosystem;
- whether an external dependency is naturally expected;
- source version/commit where applicable;
- any preprocessing or adaptation from a prior benchmark;
- license/usage terms when relevant.

Prompt text used in an experiment must be recoverable exactly from the artifact or an immutable source/version.

## 4. Dependency-Extraction Provenance

Each extracted dependency observation should preserve:

| Field | Meaning |
|---|---|
| `run_id` | source generation |
| `raw_reference` | exact string from output |
| `evidence_type` | install command / explicit list / manifest / reconciled import / unresolved module |
| `normalized_name` | ecosystem-normalized candidate |
| `ecosystem` | PyPI or npm |
| `mapping_status` | direct / mapped / ambiguous / unresolved / excluded |
| `mapping_rule` | mapping source/rule when applicable |
| `exclusion_reason` | stdlib, builtin, local, relative, virtual module, etc. |
| `extractor_version` | code/version identifier |
| `normalizer_version` | code/version identifier |

Raw evidence must be retained so a later reviewer can inspect how a normalized package observation was produced.

## 5. Registry Observation / Snapshot Record

For every registry observation used as ground truth, record:

| Field | Requirement |
|---|---|
| `ecosystem` | PyPI or npm |
| `snapshot_id` | stable local identifier |
| `acquisition_timestamp` | timezone-aware timestamp |
| `source` | endpoint/file/source description |
| `source_version` | version/revision when available |
| `sha256` | checksum for retained snapshot/artifact where practical |
| `package_count` | number of normalized package names |
| `normalizer_version` | normalization code/version |
| `filter_description` | applied filtering, if any |
| `terms_note` | source terms/license/usage note |

If only a live point lookup is possible, record the exact lookup timestamp and response status instead of pretending a historical full snapshot exists.

## 6. Public-Control Inventory

Maintain a versioned table for every proposed control:

- registry;
- control name;
- authoritative source/reference;
- date the source was checked;
- publicly documented (`yes/no/partial`);
- deterministic from external observation (`yes/no/partial`);
- reproducible externally (`yes/no/partial`);
- operationalization version;
- included in core metric (`yes/no`);
- exclusion rationale when not included.

A change in operationalization should create a new version rather than silently changing old labels.

## 7. Manual Validation Sample

Before the full experiment, create a manually reviewed sample covering:

- each evidence type;
- both ecosystems;
- standard-library/built-in exclusions;
- known import-to-distribution mismatches;
- scoped names;
- ambiguous/unresolved mappings;
- both valid and nonexistent package observations.

Record the adjudication guidelines and reviewer decisions. If practical, report extraction precision/recall or at minimum an error audit with denominator and error categories.

## 8. Analysis Reproducibility

Record:

- analysis script/notebook version;
- Python/R/runtime version;
- dependency lockfile or environment export;
- random seeds where applicable;
- exact input snapshot IDs;
- exact model run IDs included/excluded;
- exclusion criteria;
- statistical package versions;
- figure-generation commands/scripts.

All reported tables/figures should be regenerable from versioned scripts plus the releasable dataset/artifact.

## 9. Temporal Analysis

For RQ3, every comparison must identify the timepoints being compared. Suggested fields:

- `timepoint_id`;
- model/version set;
- registry snapshot IDs;
- prompt-set version;
- extraction/normalization versions;
- date range.

If processing logic changes between timepoints, rerun older data with the new logic when possible or report the incompatibility explicitly.

## 10. Repository Data Layout

Recommended structure for the later experiment:

```text
data/
├── tasks/
│   ├── prompts.jsonl
│   └── prompt_manifest.json
├── models/
│   ├── generations/
│   └── run_manifest.jsonl
├── registries/
│   ├── pypi/
│   ├── npm/
│   └── snapshot_manifest.jsonl
├── extracted/
│   └── dependency_observations.jsonl
├── validation/
│   └── manual_review.csv
└── release/
    └── README.md
```

The exact file format can change during the pilot, but provenance fields must not be dropped.

## 11. Artifact Release Policy

Public artifact goals:

- release code, prompts/tasks where licensing permits, schemas, configuration, and aggregate results;
- release exact model/version metadata and registry snapshot provenance;
- document third-party data that must be re-fetched rather than redistributed;
- avoid releasing exact identifiers when advisors/platform contacts determine that public release would create unnecessary risk;
- if identifiers are withheld, provide aggregate statistics, stable transformed IDs/hashes where appropriate, and enough pipeline detail for scientific review.

## 12. Reproduction Checklist

Before reporting final results, verify:

- [ ] every generation has a model/version/timestamp/config record;
- [ ] every prompt has stable provenance;
- [ ] every extracted dependency has evidence provenance;
- [ ] stdlib/builtin/local exclusions are versioned;
- [ ] import-to-distribution mappings are versioned;
- [ ] every registry result has an observation/snapshot timestamp;
- [ ] retained snapshots have hashes where practical;
- [ ] public-control operationalizations cite authoritative sources;
- [ ] manual validation has been completed before the full sweep is trusted;
- [ ] analysis scripts record exact input versions;
- [ ] figures/tables can be regenerated;
- [ ] restrictions/redactions are documented;
- [ ] the final paper's limitations match the actual reproducibility constraints.
