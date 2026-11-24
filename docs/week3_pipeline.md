# Week 3 — V1 Data Pipeline
**Project:** UTM Academic Advisor | **Pathway:** A — PDF/TXT documents

## Pipeline stages
1. **Ingest:** download approved PDFs and upload to Colab data/raw/.
2. **Verify:** recursively list source files; require PDF/TXT and record sizes.
3. **Inspect:** extract page-by-page; record page count, extracted character count and empty-text pages.
4. **Prepare:** normalise whitespace conservatively; compare fixed-size and paragraph-aware chunks; retain page provenance.
5. **Validate:** reject empty chunks, malformed JSONL, missing source IDs/hashes and empty output; compare export count to records.
6. **Store:** write prepared_text.jsonl and pipeline_manifest.json under data/processed/.
7. **Lineage:** record source SHA-256, filename, page, chunk ID, configuration, output hash and UTC timestamp.
8. **Repeatability:** rerun on unchanged sources and compare prepared-record fingerprints and source/output mappings.

## Failure points
| Failure | Detection | Handling |
|---|---|---|
| Missing/wrong source path | Explicit selection and existence check | Stop clearly; never silently choose an arbitrary file. |
| Scanned/image-only PDF page | Empty text extraction | Record page for manual OCR/review; do not claim it was ingested. |
| Corrupt PDF | Parser exception | Report filename/error; document exclusion. |
| Empty/malformed chunks | Validation | Fail instead of exporting empty records. |
| Source changed | SHA-256 mismatch | Regenerate prepared output and manifest. |
| Duplicate documents | Same SHA-256 | Flag for review; do not silently merge distinct editions. |

## Execution evidence
Notebook prints input paths/sizes, per-document page/extraction summaries, chunk counts, export path, validation status, manifest path and repeatability result. Actual counts/outcomes must come from a real Colab run; this repository does not claim execution against the source PDFs yet.
