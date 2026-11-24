# Week 2 — V1 Architecture Decisions and Traceability
**Project:** UTM Academic Advisor | **Version:** 1.0

## Architecture needs
| Requirement | Need | Architecture response |
|---|---|---|
| FR1 / NFR1 | Relevant academic-regulation evidence | Approved source collection, parser, chunk store and retrieval. |
| FR2 / RAI1 | Verifiable exact source | Preserve title, edition, page/section and chunk ID through retrieval/generation. |
| FR3 | Malay and English | Language-tolerant query/text path; evaluate languages separately. |
| FR4 / RAI2 | Refuse missing evidence/out-of-scope queries | Scope and evidence-sufficiency check before generation; refusal/redirect path. |
| RAI3 | Minimise student data | No default conversation persistence; redact sensitive input before external inference. |
| NFR2 | Latency target | Instrument query, retrieval and generation stages. |
| NFR3 / NFR4 | Repeatable updates | Versioned source inventory, deterministic preparation, checksums and manifest. |

## V1 conceptual architecture
See week2_architecture_v1.svg. Components are labelled by function; specific technologies are intentionally deferred.

## Architecture Decision Records
| Decision | Requirement / constraint | Alternatives considered | Selected option | Trade-off accepted |
|---|---|---|---|---|
| ADR-01 Knowledge access | FR1, FR2, NFR1, RAI1 require approved evidence and citations. | Base model only; static FAQ; retrieval over approved documents. | Retrieval over curated, versioned UTM documents with source metadata carried into responses. | More ingestion/index maintenance, but stronger grounding and traceability. |
| ADR-02 Scope/evidence control | FR4, FR5, RAI2 require safe refusal. | Prompt instructions only; explicit scope/evidence check and refusal path. | Explicit scope and evidence-sufficiency control before definitive generation. | Some valid questions may be conservatively refused; extra processing. |
| ADR-03 Provenance | FR2, RAI1, NFR4 require source edition/location. | Plain text only; metadata per document/chunk. | Store source ID, filename/title, edition/date, page/section and content hash. | More metadata and careful parsing, but auditable citations/updates. |
| ADR-04 Privacy | RAI3: student records are not needed for regulation lookup. | Persist all conversations; no default conversation persistence and redact sensitive input. | Data-minimised interaction; synthetic test data only. | Less personalisation/diagnostic logging, lower privacy exposure. |

## Initial requirements traceability matrix
| Objective | Requirement | Evaluation criterion | Architecture element | Later evidence |
|---|---|---|---|---|
| O1 | FR2 / RAI1 | Citation presence and support | Document metadata, chunk store, retrieval, citation formatter | Citation audit with page/source verification |
| O2 | FR1 / NFR1 | Correct source-supported answer rate | Retrieval + generation | ≥30 scored supported questions |
| O2 | FR4 / RAI2 | Refusal-and-redirect rate | Scope/evidence control + refusal response | ≥20 unsupported/out-of-scope prompts |
| O2 | FR3 | Malay/English correctness | Language-tolerant query/generation | Per-language test results |
| O3 | NFR2 | p90 latency | Application, retrieval and model path | Timed workload log |
| O1 | NFR4 | Repeatable output and lineage | Ingestion/preparation + manifest | Two-run hash/count/mapping comparison |

## Final V1 review
- Each major component maps to requirements or evaluation.
- Knowledge retrieval is needed for edition-specific regulation citations.
- Scope/evidence controls reduce risk of invented advice.
- Architecture remains conceptual; model, vector database, framework and deployment are deferred.
- Evaluation covers correctness, citations, refusal, bilingual behaviour and latency.

### Reflection
ADR-01 has the most important trade-off: retrieval adds preparation and maintenance complexity, but FR2 and RAI1 require traceability to approved regulation passages.
