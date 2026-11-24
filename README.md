UTM Academic Advisor
A retrieval-augmented generation (RAG) assistant that answers undergraduate students' questions about UTM academic regulations (registration, course withdrawal, academic standing, credit transfer) and cites the regulation it used.
Author: Muhammad Zarul Hazman bin Abd Halim
Course project: AI Engineering (semester project)
> Status: Week 1 — project scoping. Full details in [`docs/week1_project_scope.md`](docs/week1_project_scope.md).
Problem Statement
Academic rules are spread across long PDFs and faculty web pages. Students struggle to find the right rule, and advisers answer the same routine questions repeatedly. Mistakes can be costly, for example failing to register courses in the stated period can lead to termination. A RAG assistant can retrieve the relevant passage and give a cited answer instead of relying on a model's memory.
Evidence of the Problem
Advising struggles with scalability, consistency and changing information (Sule, 2026) — https://scholars.wlu.ca/etd/2892
Limited advisor availability delays support; students valued fast answers to routine questions in a 20-student pilot — https://openaccess.cms-conferences.org/publications/book/979-8-950676-12-3/article/979-8-950676-12-3_1
UTM FCEE guideline: failing to register in the stated period without valid reason leads to termination — https://fkt.utm.my/?p=13196
Most student enquiries at Gothenburg were already answerable from official websites (Börjesson et al., 2025) — https://odr.chalmers.se/items/d51eab9a-9519-4447-8494-af88a169b73b/full
Initial Related Work
Work	Limitation for this project
Lewis et al. (2020), RAG — https://arxiv.org/abs/2005.11401	General method over Wikipedia, not an advising system
Chen et al. (2024), admission chatbot with RAG — https://doaj.org/article/1def23f23c554d11a962e11b2399a49e	Admissions only; small test (53 students)
MARAUS multi-agent RAG for admissions — https://arxiv.org/pdf/2507.11272	Admissions domain; complex architecture
AI Academic Advisor pilot (AHFE), DOI 10.54941/ahfe1008077	Small pilot (20 students); usability focus
Engineering Gap / Limitation
Existing work focuses on admissions or small usability pilots. This project targets enrolled UTM undergraduates, with (1) cited answers from versioned regulations, (2) Malay and English support, and (3) a repeatable evaluation set with an LLM-only baseline.
Initial Objectives
Build a RAG pipeline over public UTM academic regulation documents that returns cited answers.
Evaluate with at least 30 question/answer pairs (retrieval accuracy, faithfulness) against an LLM-only baseline.
Make the system refuse safely when evidence is missing and point to official offices.
Target Users
Primary: UTM full-time undergraduate students.
Secondary: academic advisers and faculty administrative staff.
Proposed Data Sources
ID	Document	URL
D1	UTM Academic Regulations (16th ed.) official page	https://ugs.utm.my/academic-rules
D2	FT UG Academic Regulations 16th edition (PDF)	https://studentppi.utmspace.edu.my/wp-content/uploads/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf
D3	Academic Circular No. 6/2022	https://dvcai.utm.my/wp-content/uploads/2022/10/PA-Bil.-6.2022-Pemakaian-Peraturan-Akademik-Program-Sarjana-Muda-Pengajian-Sepenuh-Masa-Edisi-Keenam-Belas.pdf
D4	FCEE course registration guideline	https://fkt.utm.my/?p=13196
Only public documents are used. No personal data, credentials or API keys are committed.
Repository Structure
```
ai-engineering-utm-academic-advisor/
├── README.md
├── docs/
│   ├── week1_observation_sheet.md
│   ├── week1_system_map.png
│   ├── week1_project_scope.md
│   ├── week1_part_a_evidence.pdf
│   └── project_system_map_utm_advisor.png
├── data/
├── src/
├── notebooks/
├── evaluation/
└── prompts/
```
Weekly Progress
Week	Focus	Status	Notes
1	Project scoping, GitHub setup, observation sheet and system map	Done	Scope written; repository structure created
2	Requirements, evaluation criteria, V1 architecture and traceability	Done (documentation)	See docs/week2_* and docs/week2_architecture_v1.svg
3	PDF ingestion, preparation, validation and lineage	Notebook prepared; execution pending	See notebooks/week3_pipeline.ipynb and docs/week3_data_inventory.md


## Week 2 — Requirements and V1 architecture
- `docs/week2_requirements.md`: refined project framing, application scope, functional, non-functional and Responsible AI requirements.
- `docs/week2_evaluation_criteria.md`: measurable evaluation criteria and proposed thresholds.
- `docs/week2_architecture_v1.svg`: V1 conceptual architecture diagram (vector format).
- `docs/week2_architecture_decisions.md`: architecture needs, decision records, trade-offs and traceability matrix.

## Week 3 — Data engineering
- `docs/week3_data_inventory.md`: approved UTM source inventory and requirement-to-data mapping.
- `notebooks/week3_pipeline.ipynb`: Colab notebook for PDF/TXT ingestion, page-level inspection, chunking, validation, JSONL export, SHA-256 lineage and repeatability smoke test.
- `docs/week3_pipeline.md`: pipeline stages and failure handling.\n- `docs/week3_pipeline_diagram.svg`: visual pipeline flow.
- **Execution status:** notebook has been added but has not yet been run against the downloaded source PDFs. No dataset counts or successful execution results are claimed.
- **Data handling:** do not commit source regulation PDFs unless their terms explicitly permit redistribution.
