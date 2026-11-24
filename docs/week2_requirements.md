# Week 2 — Requirements Specification and Application Scope
**Project:** UTM Academic Advisor | **Version:** 1.0

## Section 0 — Project framing and application scope

### Problem and evidence
UTM undergraduate academic rules are distributed across lengthy regulation PDFs, academic circulars and faculty webpages. Students may struggle to identify the applicable rule, edition or deadline, while advisers and administrative staff repeatedly answer routine questions. A wrong interpretation can have serious consequences: the Week 1 scope records that the FCEE course-registration guideline warns that failure to register within the stated period without a valid reason may lead to termination.

Evidence retained from Week 1:
- Sule (2026), academic-advising thesis: scalability, consistency and changing institutional information. https://scholars.wlu.ca/etd/2892
- Design and Evaluation of an AI Academic Advisor (AHFE): small 20-student pilot and value of fast routine responses. https://openaccess.cms-conferences.org/publications/book/979-8-950676-12-3/article/979-8-950676-12-3_1
- UTM FCEE course-registration guideline: https://fkt.utm.my/?p=13196
- Börjesson et al. (2025), Chalmers thesis: many student enquiries could be answered from official websites. https://odr.chalmers.se/items/d51eab9a-9519-4447-8494-af88a169b73b/full

### Existing approaches
| Approach | What it addresses | Relevant limitation |
|---|---|---|
| Lewis et al. (2020), RAG, https://arxiv.org/abs/2005.11401 | Retrieval from external knowledge combined with generation for knowledge-intensive QA. | General method, not UTM edition-aware academic advising. |
| Chen et al. (2024), university-admission chatbot, https://doaj.org/article/1def23f23c554d11a962e11b2399a49e | LLM/RAG for university admission information. | Admission focus, not rules for enrolled UTM undergraduates. |
| MARAUS, https://arxiv.org/abs/2507.11272 | Multi-agent RAG for university admissions counselling. | Admissions domain and comparatively complex for a semester V1. |
| Design and Evaluation of an AI Academic Advisor, DOI 10.54941/ahfe1008077 | Academic-advisor chatbot and small student usability pilot. | Small pilot; Week 1 review notes limited repeatable accuracy/faithfulness evaluation. |

### Project / engineering gap
A practical engineering gap remains: students need answers grounded in the correct edition of UTM regulations, with traceable section/page references, bilingual Malay/English interaction, and repeatable evaluation of retrieval, answer support and refusal. This is not a claim of scientific novelty.

### Objectives
- **O1 — Design:** Design a traceable knowledge/retrieval workflow over public UTM undergraduate academic regulations, preserving document edition and page provenance.
- **O2 — Develop:** Develop a bilingual RAG assistant that answers in-scope academic-regulation questions with verifiable source references and refuses unsupported/out-of-scope requests.
- **O3 — Evaluate:** Evaluate at least 30 curated question/answer cases for retrieval quality, answer correctness/faithfulness, citation support and refusal; compare with an LLM-only baseline where feasible.

### Application scope
- **Intended users:** UTM full-time undergraduate students; secondary users are academic advisers and faculty administrative staff.
- **Supported tasks:** Explain rules on course registration, course withdrawal, academic standing/CGPA, credit transfer and other topics explicitly covered by approved sources; locate relevant clauses; explain source edition and page/section.
- **Knowledge domain:** Public UTM full-time undergraduate academic regulations, relevant academic circulars and explicitly approved faculty guidance.
- **Operating context:** Informational student-support assistant; Malay or English natural-language queries.
- **Constraints:** Public/permitted documents only; record edition/version; bilingual text and tables may complicate parsing; no student-record integration; semester-project compute/API budget; not a replacement for official advice.
- **Out of scope:** Individual eligibility decisions, waiver/appeal outcomes, legal/medical/financial advice, private student records, unofficial policy interpretation and answers unsupported by indexed sources.
- **Evaluation focus:** Can the system retrieve the applicable passage, generate an answer supported by it, cite it accurately, meet latency target and refuse safely when evidence is missing?

## Section 1 — Functional requirements
| ID | Requirement — The system SHALL | Scope boundary — SHALL NOT |
|---|---|---|
| FR1 | Answer natural-language questions about undergraduate academic rules covered by approved UTM sources. | Invent rules or imply an unindexed topic is covered. |
| FR2 | Present supporting regulation passages with document title, edition/version and page or section reference where available. | Present uncited regulation claims as verified. |
| FR3 | Support Malay and English questions and responses, including retrieval from approved Malay/English content. | Claim equivalent language quality until both languages are evaluated. |
| FR4 | State when sufficient evidence was not found and direct users to an appropriate official UTM office/source. | Guess from model memory or make an official decision. |
| FR5 | Restrict answers to UTM undergraduate academic regulations and explain the informational-only role. | Make personal eligibility decisions or provide unrelated professional advice. |

## Section 2 — Non-functional requirements
Thresholds are proposed V1 targets, not achieved results until tested.

| ID | Quality attribute | Requirement | Measurement method | Metric | Threshold |
|---|---|---|---|---|---|
| NFR1 | Correctness | Answers shall be consistent with approved source passages. | Human-score at least 30 curated cases against answer key and source. | Correct, source-supported answer rate | ≥ 80% |
| NFR2 | Performance | Interactive answers shall complete within target response time. | Measure end-to-end latency on a defined workload. | p90 latency | < 3,000 ms target; report conditions |
| NFR3 | Maintainability | Approved source updates shall be incorporated through documented ingestion without rebuilding unrelated components. | Time a controlled add/replace-document test. | Update time | < 10 minutes for data preparation, excluding manual review |
| NFR4 | Reliability | Pipeline shall produce valid, non-empty records and a lineage manifest or fail clearly. | Run twice on unchanged inputs and compare outputs. | Repeatability | 2/2 runs; identical content fingerprints |

## Section 3 — Responsible AI requirements
| ID | Risk | Requirement | Measurement method | Metric | Threshold |
|---|---|---|---|---|---|
| RAI1 | Transparency / grounding | Every document-grounded answer shall include a verifiable source reference and indicate when evidence is insufficient. | Inspect supported answers and verify cited passages. | Citation presence; citation-source support | 100% presence; ≥ 95% supported |
| RAI2 | Safety / scope | Refuse unsupported or out-of-scope questions and provide an official redirect where possible. | Test at least 20 out-of-scope/unsupported prompts. | Correct refusal-and-redirect rate | ≥ 95% |
| RAI3 | Privacy | Do not intentionally collect or persist student identifiers/private records; exclude or redact sensitive input before external model/API transmission. | Inspect synthetic test payloads and stored logs. | Unredacted PII transmissions / persistent PII records | 0 |

### Reflection
RAI1 is hardest to measure: citation presence is easy to count, but a citation may not support the generated claim. Citation-source support must be checked against the actual cited passage, not inferred from a link alone.
