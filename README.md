# GrantGuide: An AI Assistant for Malaysian Grants and Startup Funding

GrantGuide is a retrieval-augmented generation (RAG) assistant that helps Malaysian SMEs and student entrepreneurs find public grants and startup funding. Users describe their business in plain language, and GrantGuide returns matching programmes with eligibility details, programme type (grant or loan), and citations to official sources.

> Information only. Not financial advice. Always verify with the funding agency.

Full details: [docs/week1_project_scope.md](docs/week1_project_scope.md)

## Problem Statement

Malaysian funding programmes are spread across agency websites, PDFs, and news releases, each describing eligibility differently (turnover, headcount, ownership, MSME status). Founders must work out for themselves which programmes fit, and they often mistake loans for grants. Keyword search and fixed filters cannot match a free-text business description to eligibility rules written in varied wording, so an AI approach that retrieves and explains the relevant passages is appropriate.

## Evidence of the Problem

1. An ACCCIM survey of 682 SMEs found that 67% did not understand how to tap into government soft loans, grants, and tax incentives. ([Malaysiakini](https://www.malaysiakini.com/news/66006)) 
2. A 2026 analysis found that SME Corp's RMK-13 funding is financing, not grants, and that a rebate of up to 30% must be applied for separately through MyBPI. ([Newswav](https://newswav.com/article/sme-corp-rmk-13-funding-2026-rm117m-is-a-loan-not-a-grant-A2608_7glkUm))

## Initial Related Work

| Solution | Source | Key limitation |
|----------|--------|----------------|
| SME Corp MyBPI portal | https://smecorp.gov.my | Organised by programme. No personalised matching or natural-language Q&A. |
| Singapore GoBusiness grant finder | https://www.gobusiness.gov.sg | Fixed questionnaires, no free-text input or citations, Singapore only. |
| UK Business finance support finder | https://www.gov.uk/business-finance-support | Filter-based, with no explanation of why a scheme matches. Not Malaysian. |

## Engineering Gap / Limitation

No existing tool answers free-text eligibility questions for Malaysian programmes while citing source passages and flagging uncertainty. The challenges are heterogeneous documents (HTML and PDF), fast-changing programmes (staleness), LLM hallucination of amounts and deadlines, and distinguishing grants from loans.

## Project Objectives

- **O1:** To build a RAG pipeline over at least 3 official Malaysian funding sources that answers eligibility questions with passage-level citations.
- **O2:** To extract key programme attributes (type, amount, eligibility, deadline) so grants and loans are clearly distinguished.
- **O3:** To evaluate the system on a labelled set of 25-30 questions for answer correctness, citation accuracy, and abstention behaviour.

## Target Users

- Student entrepreneurs and early-stage founders
- Owners of micro, small, and medium enterprises
- University incubator and entrepreneurship office staff

## Proposed Data Sources

| Source | URL |
|--------|-----|
| SME Corp Malaysia (programmes, financing, PDFs) | https://smecorp.gov.my |
| MDEC (digital economy and startup programmes) | https://mdec.my |
| Cradle Fund (startup funding) | https://www.cradle.com.my |

## Weekly Progress

| Week | Completed | Next steps |
|------|-----------|------------|
| 1 | Chose topic, drafted project scope (`docs/week1_project_scope.md`), created repo and README | Verify source URLs, download documents, find a recent (2022+) evidence source |
| 2 | | |
| 3 | | |

_This README is updated weekly as the project progresses._
