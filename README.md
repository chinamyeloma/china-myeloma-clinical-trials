# China Multiple Myeloma Clinical Trials — Open Dataset

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22690814.svg)](https://doi.org/10.5281/zenodo.22690814)

A machine-readable dataset of multiple myeloma clinical trials registered in China, curated from official NMPA / CDE filings by the [China Myeloma Digital Network (CMDN)](https://chinamyeloma.org), an independent non-profit patient advocacy organisation.

Copyright © 2026 China Myeloma Digital Network (chinamyeloma.org).
Released under the [Creative Commons Attribution 4.0 International License](LICENSE) (CC BY 4.0) — use it, redistribute it, build on it, commercially or not. The one condition is attribution: credit CMDN and link back. See [How to cite](#how-to-cite).

## What this is, and why it exists

International trial registries cover China poorly. The national registration platform ([chinadrugtrials.org.cn](https://www.chinadrugtrials.org.cn/)) is a search interface with no permanent per-study URL and no machine-readable export, so Chinese myeloma trials are effectively invisible to anyone building trial-matching tools, writing systematic reviews, or advising a patient from outside the country.

This dataset closes that gap: one record per trial, normalised fields, deduplicated centre names, and an archived PDF of the original filing behind every record.

## Snapshot

| | |
|---|---|
| Snapshot date | **2026-09-10** |
| Trials | **71** (70 recruiting) |
| Deduplicated study centres | **554** (China 337, overseas 217) |
| Chinese provincial-level regions covered | **31** |
| Trial × centre rows | 1510 |
| Registry status | `cde_registered` 65 · `international_registry_only` 4 · `other_identifier_only` 1 · `unregistered` 1 |
| Records with an archived source PDF | 71 / 71 |
| Records cross-registered elsewhere | 39 / 71 |

Every figure above is rendered from the snapshot itself, not typed in by hand.

Centre counts cover recruiting studies only, and are computed **after** institution-name normalisation. Read them from `summary` in the JSON rather than aggregating records yourself — [Counting conventions](docs/methodology.md#counting-conventions) explains why that distinction changes the number.

## Files

| File | What it holds |
|---|---|
| `data/clinical-trials.json` | Complete dataset: `generatedAt`, `summary`, and one object per trial |
| `data/clinical-trials.csv` | One row per trial (25 columns), arrays flattened, eligibility bounds promoted to columns |
| `data/trial-centers.csv` | Long format: one row per trial × centre (1510 rows), with province and China/overseas resolution |
| `docs/field-dictionary.md` | Every field, its type, and its permitted values |
| `docs/methodology.md` | How records are extracted, normalised and counted |
| `CHANGELOG.md` | Per-snapshot summary, newest first |

CSV files carry a UTF-8 BOM so Excel renders Chinese centre names correctly.

## Provenance

Every record traces back to a primary document.

- `source_pdf_url` links to the archived original filing, hosted by CMDN at `chinamyeloma.org/registry/`. **This is the citable source**, because the originating NMPA/CDE platform has no permanent per-study URL. Present on 71 of 71 records.
- `cross_registry_ids` lists the same study's identifiers in other public registries (chiefly ClinicalTrials.gov) with direct verification links — the machine-fetchable way to cross-check a record against an independent registry. Present on 39 of 71 records.
- `registry_status` records how well-registered a study is: `cde_registered` 65 · `international_registry_only` 4 · `other_identifier_only` 1 · `unregistered` 1. **A study whose `registry_status` is not `cde_registered` must not be described as NMPA-registered.**

## Known limitations

Stated plainly, because a dataset you can't judge is a dataset you shouldn't cite.

- **Snapshot, not a live feed.** Each release is a point-in-time capture. Trial status changes between releases.
- **Myeloma only, China-registered trials only.** Not a general oncology registry. Hong Kong, Macau and Taiwan count as Chinese provincial-level regions in centre tallies.
- **Titles and centre names are in Chinese**, as filed. No machine translation is applied, because translating institution names introduces ambiguity that dedupe cannot recover from.
- **`population` is derived by CMDN**, not an official CDE field, and mixes granularities — it currently takes the values `复发难治` 53 · `多发性骨髓瘤` 12 · `健康受试者` 2 · `新诊断` 2 · `新诊断/复发难治` 1 · `新诊断/维持治疗` 1.
- **`therapy_types` uses a mixed vocabulary.** Labels are currently `monoclonal_antibody` 17 · `bispecific_antibody` 17 · `CAR-T` 16 · `small_molecule` 9 · `antibody_drug_conjugate` 3 · `三抗` 2 · `cell_therapy` 1 · `ADC` 1 · `单抗` 1: some Chinese, some English snake_case, and `ADC` / `antibody_drug_conjugate` denote the same modality. Match on the set, not on a single spelling.
- **`eligibility` is extracted from filing PDFs**, then human-reviewed. 11 of 71 records have a non-empty `verification.unmatched`, and 1 has no PDF text layer at all. Check that field before relying on eligibility.
- **Overseas centre counts** for multi-national studies reflect what the Chinese filing lists, which is not always the study's full global footprint.

## How to cite

Cite the **concept DOI** below — it always resolves to the latest version, and Zenodo lists every prior version from that landing page. If your work depends on one exact snapshot, take that snapshot's own version DOI from the Zenodo record instead.

**APA**

> China Myeloma Digital Network. (2026). *China multiple myeloma clinical trials — open dataset* (Version 2026-09) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.22690814

**BibTeX**

```bibtex
@dataset{cmdn_china_myeloma_trials,
  author    = {{China Myeloma Digital Network}},
  title     = {China Multiple Myeloma Clinical Trials --- Open Dataset},
  year      = {2026},
  version   = {2026-09},
  publisher = {China Myeloma Digital Network},
  doi       = {10.5281/zenodo.22690814},
  url       = {https://doi.org/10.5281/zenodo.22690814}
}
```

GitHub also renders a **Cite this repository** button from [`CITATION.cff`](CITATION.cff).

## Related

- Dataset documentation and field dictionary on the web: https://chinamyeloma.org/en/clinical-trials/dataset
- Browsable trial records: https://chinamyeloma.org/en/clinical-trials
- Editorial and sourcing policy: https://chinamyeloma.org/en/editorial-policy
- Archived releases and version DOIs: https://zenodo.org/records/22690814

## Disclaimer

This dataset is research and navigation information. It is not medical advice, and inclusion of a trial is neither a recommendation nor a statement that any patient is eligible. Enrolment decisions belong to the treating physician and the study's investigators.

Interventional clinical trials in mainland China almost exclusively enrol domestic residents holding national identification. Presence of a trial in this dataset does not imply that international patients can enrol.

## Contact

Issues and corrections: please open a GitHub issue with the trial's `registry_id` and the discrepancy you found. Corrections are applied to the next release, not silently to a published one.
