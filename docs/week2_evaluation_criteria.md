# Week 2 — Evaluation Criteria
**Project:** UTM Academic Advisor | **Version:** 1.0  
**Status:** Proposed targets, not achieved results.

| Objective / Requirement | Evaluation criterion | Capability bucket | Measurement method | Metric | Threshold | User / business outcome |
|---|---|---|---|---|---|---|
| O3 / FR1 / NFR1 | Academic answer correctness and source consistency | Domain-specific capability | Score ≥30 supported questions against answer key and official clause. | Correct, source-supported answer rate | ≥80% | Reliable answers to routine regulation questions. |
| O1 / FR2 / RAI1 | Citation presence and evidence support | Instruction-following capability | Inspect each answer for title, edition and page/section; verify cited passage supports claim. | Citation presence; citation-source support | 100%; ≥95% | Students can verify rules against university sources. |
| O2 / FR4 / RAI2 | Safe refusal for unsupported/out-of-scope questions | Instruction-following capability | Test ≥20 prompts; score refusal and official redirect. | Correct refusal-and-redirect rate | ≥95% | Reduces risk of invented/inappropriate advice. |
| O2 / FR3 | Malay and English query handling | Generation capability | Balanced Malay/English supported questions; score clarity and grounding. | Correct response rate by language; language gap | ≥80% each; report gap | Access in either supported language. |
| O3 / NFR2 | End-to-end response latency | Cost and latency | Time a fixed workload under documented conditions. | p90 latency | <3,000 ms target | Timely routine responses. |
| O1 / NFR4 | Pipeline repeatability and lineage | Domain-specific capability | Run twice; compare source/output hashes, counts and mappings. | Repeatability pass rate | 2/2; identical prepared fingerprints | Maintainers can trace prepared text to source files. |

## Evaluation protocol
1. Freeze ≥30 supported questions with expected answer points and source clauses; include Malay and English.
2. Add ≥20 unsupported/out-of-scope prompts; score separately from supported-answer accuracy.
3. Record retrieved source IDs/pages, answer, citations, latency and human scores.
4. Score citation support separately from citation presence.
5. Run an LLM-only baseline on the same supported set where feasible.
6. Report results by language and note source coverage. Do not describe proposed thresholds as achieved.

### Reflection
Citation-source support (O1 / FR2 / RAI1) is most important: a fluent but unsupported regulation answer can mislead a student, while a verifiable passage allows checking the rule and edition.
