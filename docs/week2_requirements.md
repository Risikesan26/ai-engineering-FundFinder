# GrantGuide: Week 2 Requirements

> Preliminary. These requirements will be refined as the project progresses.

## Section 0. Project Framing

**Project:** GrantGuide, a retrieval-augmented generation (RAG) assistant for Malaysian public funding information.

**Purpose:** Help Malaysian SMEs and student entrepreneurs understand which public grants and financing programmes may fit their business, with every claim traceable to an official source.

**Core problem:** Funding information is scattered across agency websites and PDFs, eligibility wording varies, and loans are often mistaken for grants (see `docs/week1_project_scope.md`).

**Why AI:** Matching a free-text business description to eligibility rules written in varied wording is not reliably done by keyword search or fixed filters.

**Assumptions**
- A1: All source documents are publicly accessible and used within their terms of access.
- A2: V1 supports English queries. Bahasa Malaysia is a stretch goal.
- A3: The corpus is a small, manually curated snapshot of 3+ agencies, not a live crawl.
- A4: The system is informational only and does not provide financial advice.

## Section 1. Application Scope

**In scope (V1)**
- Natural-language questions about Malaysian public grants, matching grants, and financing programmes.
- Sources: SME Corp (https://smecorp.gov.my), MDEC (https://mdec.my), Cradle Fund (https://www.cradle.com.my).
- Output: candidate programmes with type, eligibility summary, citations, and source date.
- Offline ingestion pipeline and an evaluation harness with a labelled question set.

**Out of scope (V1)**
- Submitting applications or filling forms.
- Certifying or guaranteeing eligibility, or predicting approval chances.
- Tax, legal, or investment advice.
- Private bank products, crowdfunding, and venture capital.
- Real-time monitoring of programme changes.
- Handling personal identifiers or uploaded company documents.

**Target users:** Student entrepreneurs and early-stage founders, SME owners, and university incubator staff.

## Section 2. Functional Requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-1 | The system shall accept a free-text description of a business and a funding question. | Must |
| FR-2 | The system shall extract a business profile (entity type, turnover, headcount, sector, stage) and ask one clarifying question when a key field needed for eligibility is missing. | Should |
| FR-3 | The system shall retrieve relevant passages from the indexed corpus for each query. | Must |
| FR-4 | The system shall return candidate programmes with name, agency, programme type (grant, matching grant, loan, other), and an eligibility summary. | Must |
| FR-5 | The system shall cite the source passage and URL for every factual claim (amount, eligibility, deadline). | Must |
| FR-6 | The system shall display the retrieval date of each cited source. | Must |
| FR-7 | The system shall abstain and state uncertainty when retrieved evidence is insufficient or the question is out of scope. | Must |
| FR-8 | The system shall show a disclaimer on every response and use "may be eligible" wording instead of guarantees. | Must |
| FR-9 | The system shall provide a re-runnable ingestion pipeline (collect, parse, chunk, embed, index) that records source URL and retrieval date. | Must |
| FR-10 | The system shall log queries and outputs without personal data for evaluation. | Should |

## Section 3. Non-Functional and Responsible AI Requirements

### 3.1 Non-Functional Requirements

| ID | Category | Requirement |
|----|----------|-------------|
| NFR-1 | Accuracy | Answers shall be factually consistent with the cited sources (see evaluation criteria). |
| NFR-2 | Performance | End-to-end response time shall be at most 10 seconds at the 95th percentile on a single user. |
| NFR-3 | Reproducibility | The pipeline shall be reproducible from the repository using pinned dependencies and a config file, and evaluation results shall be re-runnable. |
| NFR-4 | Maintainability | Components (collector, parser, chunker, retriever, generator, guardrail) shall be separate modules with defined interfaces so each can be changed independently. |
| NFR-5 | Cost | Running the full evaluation set shall stay within the student budget, and per-query cost shall be recorded. |
| NFR-6 | Usability | Answers shall be readable by non-specialists, with a clear programme-type label and at most about 300 words. |
| NFR-7 | Privacy | The system shall not store personal identifiers or ask for them. |

### 3.2 Responsible AI Requirements

| ID | Requirement | Addresses risk |
|----|-------------|----------------|
| RAI-1 | **Grounding and citation:** every factual claim shall be supported by a retrieved passage, and unsupported claims shall be removed or flagged. | Hallucinated or outdated details |
| RAI-2 | **Abstention:** the system shall decline when retrieval confidence is low or the question is out of scope, instead of guessing. | Hallucinated details |
| RAI-3 | **Grant vs loan clarity:** every programme shall be labelled with its type, and loans shall never be described as grants. | Loans mistaken for grants |
| RAI-4 | **No guarantees:** responses shall not state or imply approval, and shall include the disclaimer and advice to verify with the agency. | Implied approval, financial advice |
| RAI-5 | **Freshness transparency:** each answer shall show the source retrieval date and warn that programmes change. | Outdated information |
| RAI-6 | **Language and regional fairness:** evaluation shall include Bahasa Malaysia and Sabah/Sarawak queries, and results shall be reported separately. | Language and regional bias |
| RAI-7 | **Data minimisation:** inputs shall not be stored with identifiers, and users shall be warned not to enter personal information. | Privacy |
