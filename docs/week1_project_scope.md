# Week 1 — Project Scope

**Student:** Muhammad Zarul Hazman bin Abd Halim
**Programme:** Bachelor of Artificial Intelligence with Honours
**Status:** Preliminary. To be refined in later topics.

---

## 1. Project Name and Domain

- **Project name:** UTM Academic Advisor (repository: `ai-engineering-utm-academic-advisor`)
- **Domain:** Higher education, student academic advising and university regulation question answering.
- **One-line summary:** A retrieval-augmented generation (RAG) assistant that answers undergraduate students' questions about academic rules (registration, course withdrawal, academic standing, credit transfer, misconduct) and cites the exact regulation it used.

## 2. Problem Statement

Undergraduate students must follow a large set of academic rules, for example when to register courses, how to withdraw from a course, how academic standing is decided from CGPA, and how credit transfer works. These rules are published as long PDF documents and as scattered faculty web pages. Students who cannot find or interpret the right rule ask academic advisers or administrative offices, which adds repeated routine questions to staff workload and can delay answers.

The consequences of getting a rule wrong can be serious. For example, a UTM faculty registration guideline states that a student who fails to register courses within the period stated in the work schedule, without valid reasons, will be terminated from the university (see Source E3).

**Who experiences it:** undergraduate students (especially new students), and the advisers and administrative staff who answer repeated questions.

**Why AI is appropriate:** the questions are natural-language and varied, while the answers already exist in official documents. A RAG system can retrieve the relevant passage and generate a grounded answer with a citation, instead of relying on a language model's memory. This suits a knowledge base that is updated with new editions of the regulations.

## 3. Evidence of the Problem (initial)

| # | Evidence | Source |
|---|----------|--------|
| E1 | Academic advising struggles with scalability, consistency and changing institutional information; RAG-based advising systems are proposed to address this. | Sule (2026), Wilfrid Laurier University thesis — https://scholars.wlu.ca/etd/2892 |
| E2 | Limited advisor availability and growing administrative workload can delay student support; an AI advisor pilot (20 students) found students valued fast responses for routine enquiries. | Design and Evaluation of an AI Academic Advisor (AHFE), DOI 10.54941/ahfe1008077 — https://openaccess.cms-conferences.org/publications/book/979-8-950676-12-3/article/979-8-950676-12-3_1 |
| E3 | UTM FCEE guideline: course registration is mandatory, only offered courses can be registered, and failing to register in the stated period without valid reason leads to termination. | https://fkt.utm.my/?p=13196 |
| E4 | Interviews with study counsellors at the University of Gothenburg found that answers to most student enquiries were already on official websites, which supports building a chatbot over official documents. | Börjesson et al. (2025), Chalmers bachelor's thesis — https://odr.chalmers.se/items/d51eab9a-9519-4447-8494-af88a169b73b/full |
| E5 | UTM regulations are issued in numbered editions and amended through academic circulars (e.g. Academic Circular No. 6/2022 introduced the 16th edition from the 2022/2023 session), so students must track which version applies. | https://dvcai.utm.my/wp-content/uploads/2022/10/PA-Bil.-6.2022-Pemakaian-Peraturan-Akademik-Program-Sarjana-Muda-Pengajian-Sepenuh-Masa-Edisi-Keenam-Belas.pdf |

> **Next step (Week 2+):** collect first-hand evidence, such as a short survey of classmates on how they currently find rules, and a log of real questions.

## 4. Initial Related Work

| # | Work | What it does | Limitations relevant to this project |
|---|------|--------------|--------------------------------------|
| R1 | Lewis et al. (2020), *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks*, NeurIPS — https://arxiv.org/abs/2005.11401 | Introduces RAG: combining a pre-trained generator with a retriever over an external document index. | General open-domain QA over Wikipedia; it is a method, not a deployed advising system. Provenance and knowledge updating are noted as open problems. |
| R2 | Chen, Zou, Xie & Lou (2024), *Facilitating university admission using a chatbot based on LLMs with RAG*, Educational Technology & Society — https://doaj.org/article/1def23f23c554d11a962e11b2399a49e | Admission chatbot (ChatGPT 3.5 + LlamaIndex) tested on admission data from three universities, with feedback from 53 high-school students. | Focuses on admission, not rules for enrolled students; small user test; not designed for UTM documents or Malay/English mixed queries. |
| R3 | *MARAUS: An Empirical Study of Multi-Agent RAG for Real-World University Admissions Counseling* — https://arxiv.org/pdf/2507.11272 | Real-world, multi-agent RAG admissions chatbot with hybrid retrieval and LLM re-ranking. | Admissions domain and its own institution; its design is relatively complex (multi-agent) for a semester project; authors list other domains as future work. |
| R4 | *Design and Evaluation of an AI Academic Advisor* (AHFE), DOI 10.54941/ahfe1008077 | RAG-based academic advisor grounded in official documents, evaluated in a pilot with 20 students via usability survey. | Small pilot; usability focus rather than measured answer accuracy against a test set. |

## 5. Engineering Gap / Limitation

The related work mostly targets admissions or reports small usability pilots. What is less clear from the sources above is a system for **enrolled undergraduates at a Malaysian university** that:

1. Answers rule questions from **versioned official regulations** and cites section/page so students can verify.
2. Handles **Malay and English** documents and queries (the UTM circular is bilingual).
3. Is evaluated with a **repeatable test set** (retrieval hit rate, answer faithfulness, refusal when unsupported), not only user satisfaction.

My project aims to build and evaluate this narrow, well-scoped system.

## 6. Project Objectives (initial)

1. **Build** a RAG pipeline (ingest, chunk, embed, retrieve, generate) over public UTM undergraduate academic regulation documents, returning answers with source citations.
2. **Evaluate** the system with a hand-built set of at least 30 question/answer pairs, measuring retrieval accuracy and answer faithfulness, and compare against an LLM-only baseline.
3. **Design for safe use:** the assistant must say "I could not find this in the regulations" when evidence is missing and direct students to the official office for confirmation.

## 7. Target Users

- **Primary:** UTM full-time undergraduate students, especially first- and second-year students.
- **Secondary:** academic advisers and faculty administrative staff who want to reduce repeated routine questions.

## 8. Data Sources (public documents)

| ID | Document | URL |
|----|----------|-----|
| D1 | UTM Academic Regulations, Full-Time Bachelor's Programme (16th edition) — official page (Undergraduate Studies School) | https://ugs.utm.my/academic-rules |
| D2 | FT UG Academic Regulations, 16th edition (PDF) | https://studentppi.utmspace.edu.my/wp-content/uploads/2026/05/FT-UG-ACADEMIC-REGULATIONS-16th-edition.pdf |
| D3 | Academic Circular No. 6/2022 (adoption of the 16th edition) | https://dvcai.utm.my/wp-content/uploads/2022/10/PA-Bil.-6.2022-Pemakaian-Peraturan-Akademik-Program-Sarjana-Muda-Pengajian-Sepenuh-Masa-Edisi-Keenam-Belas.pdf |
| D4 | FCEE guideline for undergraduate course registration | https://fkt.utm.my/?p=13196 |

Only publicly available documents are used. No student records, grades or personal data will be collected or committed. *(Before ingesting, I will check each document's terms of use and confirm it is the current edition.)*

## 9. Comparison with EduAdvisor

**Similarities**
- Both are education-domain assistants that help students get guidance.
- Both rely on an AI model combined with curated documents rather than free-form answers.
- Both need responsible-AI safeguards because students may act on the answers.

**Differences**
- **Scope:** my project answers only questions about UTM academic regulations, not general advice.
- **Grounding:** every answer must cite a regulation section; unsupported questions are refused.
- **Language:** supports Malay and English source material.
- **Evaluation:** uses a repeatable test set with a baseline comparison.

## 10. Responsible AI Risks

| Risk | Why it matters | Planned mitigation |
|------|----------------|--------------------|
| **Incorrect or hallucinated rule answers** | A wrong answer about registration or withdrawal could cause real harm (see E3). | Answer only from retrieved text, show citations, refuse when evidence is weak, include a disclaimer pointing to official offices; measure faithfulness. |
| **Outdated or wrong-edition documents** | Regulations change by edition and circular (E5). | Record the edition and date for each document in metadata; show it with each answer; document an update process. |
| **Privacy of student data** | Students may type IDs, grades or personal situations. | Do not store conversations by default; warn users not to enter personal data; keep logs out of the repository. |
| **Over-reliance and uneven language quality** | Students may treat the bot as authoritative; Malay queries may perform worse than English. | Clear "informational only" labelling; test Malay and English questions separately and report the difference. |
