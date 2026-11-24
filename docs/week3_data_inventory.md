# Week 3 — Data Inventory
**Project:** UTM Academic Advisor  
**Selected pathway:** A — PDF/TXT document data (unstructured text).  
**Purpose:** Prepare approved UTM undergraduate academic regulation documents for later retrieval. Actual files must be placed in data/raw/ and inspected by running the notebook.

## Requirement-to-data mapping
| Requirement | Needed information | Source need |
|---|---|---|
| FR1 / NFR1 | Clauses on registration, withdrawal, academic standing and credit transfer | Official academic regulation text |
| FR2 / RAI1 | Source title, edition, page/section and clause text | Document identity and page-level provenance |
| FR4 / RAI2 | Corpus coverage and absent supporting passages | Prepared corpus and unsupported-query tests |
| NFR4 | Repeatable source update | Raw files, checksums, configuration and manifest |

## Source inventory
| ID | Filename / URL | Type and purpose | Version/date/owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf — https://studentppi.utmspace.edu.my/wp-content/uploads/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf | PDF; primary full-time undergraduate academic rules | 16th edition; confirm date from PDF cover; UTM source | Publicly accessible URL; check terms and retain attribution; do not redistribute unless permitted. | Pending notebook run: file, page count, extracted characters, empty pages and samples. |
| S02 | PA-Bil.-6.2022-Pemakaian-Peraturan-Akademik-Program-Sarjana-Muda-Pengajian-Sepenuh-Masa-Edisi-Keenam-Belas.pdf — https://dvcai.utm.my/wp-content/uploads/2022/10/PA-Bil.-6.2022-Pemakaian-Peraturan-Akademik-Program-Sarjana-Muda-Pengajian-Sepenuh-Masa-Edisi-Keenam-Belas.pdf | PDF; circular adopting the 16th edition | Circular No. 6/2022; UTM | Publicly accessible; check terms and preserve attribution. | Pending notebook run: file, page count, extracted characters and samples. |
| S03 | https://ugs.utm.my/academic-rules | Official academic-rules landing page; check current edition/status | Page may change; UTM Undergraduate Studies School | Public webpage; verify current links and terms. | Manual source check; not parsed by PDF-only V1 notebook. |
| S04 | https://fkt.utm.my/?p=13196 | Faculty guideline supporting course-registration context | FCEE, UTM; faculty-specific | Public webpage; do not generalise faculty-specific rules to all faculties. | Manual scope check; not included in initial PDF pipeline. |

## Storage and preparation decisions
- Raw originals belong under data/raw/; prepared text and manifests under data/processed/ in Colab. Do not commit source PDFs unless redistribution is permitted.
- Use PDF/TXT pathway only; this is document-grounded RAG, not image, tabular, audio or time-series processing.
- Extract text page by page and preserve original page number. Flag pages with no extractable text for manual review/OCR.
- Compare fixed-character and paragraph-aware chunking on representative extracted text. V1 uses paragraph-aware chunks with a maximum character target and records page numbers.
- Preserve source filename, source ID, SHA-256, page number, chunk ID and processing configuration.
- Do not invent file counts, extraction results, licence conclusions or data-quality scores before running on actual downloaded files.

## Inspection status
**Pending execution in Google Colab.** Download S01 and S02, confirm edition and terms, upload them to /content/week3_project/data/raw/, then run notebooks/week3_pipeline.ipynb. Replace this status with observed page counts, extraction issues and samples after execution.
