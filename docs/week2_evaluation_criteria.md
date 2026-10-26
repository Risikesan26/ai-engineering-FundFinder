# GrantGuide: Week 2 Evaluation Criteria

> Preliminary. Thresholds are initial targets and will be revisited after the first baseline run.

**Evaluation set:** 25-30 manually labelled questions built from the data sources, including: in-scope eligibility questions, grant-vs-loan questions, questions with missing information, out-of-scope questions, and a small set of Bahasa Malaysia queries. Each item has a reference answer and the source passage it should cite.

| ID | Criterion | Measurement method | Metric | Threshold (initial target) | Expected outcome | Related requirements |
|----|-----------|--------------------|--------|----------------------------|------------------|----------------------|
| EV-1 | Retrieval quality | For each labelled question, check whether the gold source passage appears in the retrieved top-k results. | Recall@5 | >= 85% | The correct passage is usually retrieved before generation. | FR-3 |
| EV-2 | Answer correctness | Compare each answer with the reference answer using a rubric (correct / partially correct / wrong), scored by the student and spot-checked by a second reviewer or LLM judge. | % correct (partial = 0.5) | >= 80% | Most eligibility answers are correct. | FR-4, NFR-1 |
| EV-3 | Citation accuracy | For each cited claim, check that the cited passage actually supports it. | Citation precision | >= 90% | Citations can be trusted and verified. | FR-5, RAI-1 |
| EV-4 | Faithfulness (hallucination rate) | Count claims in answers not supported by retrieved passages. | % unsupported claims | <= 5% | Few or no invented amounts, deadlines, or criteria. | RAI-1, NFR-1 |
| EV-5 | Programme type labelling | Compare the predicted type (grant / matching grant / loan / other) with the manually labelled type for each programme. | Classification accuracy | >= 95%, with 0 loans labelled as grants | Grants and loans are not confused. | FR-4, RAI-3 |
| EV-6 | Abstention behaviour | Run out-of-scope and insufficient-evidence questions and check the system declines or states uncertainty. | Correct abstention rate on those items; false abstention rate on in-scope items | >= 90% correct abstention; <= 15% false abstention | The system declines instead of guessing, without refusing too often. | FR-7, RAI-2 |
| EV-7 | Disclaimer and wording compliance | Automatically check every response for the disclaimer and absence of guarantee phrases. | % compliant responses | 100% | No response implies approval or gives financial advice. | FR-8, RAI-4 |
| EV-8 | Freshness transparency | Check every response with citations shows the source retrieval date. | % responses with dates | 100% | Users can see how old the information is. | FR-6, RAI-5 |
| EV-9 | Clarifying behaviour | Run questions missing key profile fields and check the system asks one relevant clarifying question. | % appropriate clarifications | >= 70% | Missing information is requested, not assumed. | FR-2 |
| EV-10 | Latency | Time end-to-end responses on the evaluation set. | p95 response time | <= 10 s | The assistant feels usable in a chat setting. | NFR-2 |
| EV-11 | Language and regional fairness | Score the Bahasa Malaysia and Sabah/Sarawak subsets with EV-2 and EV-3 and compare with the English baseline. | Accuracy gap vs overall | Gap <= 15 percentage points (report, not gate, in V1) | Weaknesses in non-English or regional queries are measured and reported. | RAI-6 |
| EV-12 | Reproducibility | Re-run the ingestion and evaluation from a clean checkout. | Same corpus counts and metrics within tolerance | Metrics within 3 percentage points across runs | Results can be reproduced by others. | FR-9, NFR-3 |
| EV-13 | Cost | Record token usage and cost per query and per full evaluation run. | USD per query and per run | Recorded for every run; budget set after baseline | Cost is known and controlled. | NFR-5 |
| EV-14 | Privacy | Review logs and code for stored identifiers. | Number of stored identifiers | 0 | No personal data is retained. | FR-10, NFR-7, RAI-7 |

## Reporting

- Every evaluation run records: date, corpus snapshot, model and prompt versions, and all metrics above.
- Results are stored in the repository (for example `eval/results/`) and summarised in the weekly progress table.
- A baseline (plain keyword search or LLM without retrieval) will be run on the same set so improvements can be shown.
