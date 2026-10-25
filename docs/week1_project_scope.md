# GrantGuide: Week 1 Project Scope

## 1. Project Name and Domain

**Project:** GrantGuide, a retrieval-augmented generation (RAG) assistant for public funding information.
**Domain:** Government grants and startup financing for Malaysian SMEs and student entrepreneurs.

> GrantGuide provides information only. It is not financial advice and does not certify eligibility or submit applications.

## 2. Problem Statement

Malaysian funding programmes are spread across agency websites, PDFs, and news releases, each describing eligibility differently (turnover, headcount, ownership, MSME status). Founders and student entrepreneurs must work out for themselves which programmes fit, and they often mistake loans for grants.

**Why AI:** Keyword search and fixed filters cannot match a free-text business description ("two-person Sdn Bhd, RM300k turnover, building an app") to eligibility rules written in varied wording. A RAG system can interpret the description, retrieve the relevant passages, and explain each match with citations.

## 3. Evidence of the Problem

1. An ACCCIM survey of 682 SMEs found that 67% did not understand how to tap into government soft loans, grants, and tax incentives. (Malaysiakini, https://www.malaysiakini.com/news/66006). *Note: undated and likely old, so a recent source will be added.*
2. A 2026 analysis found that SME Corp's RMK-13 funding is financing, not grants, and that a rebate of up to 30% must be applied for separately through MyBPI. (Newswav, https://newswav.com/article/sme-corp-rmk-13-funding-2026-rm117m-is-a-loan-not-a-grant-A2608_7glkUm)

## 4. Initial Related Work

| Solution | Source | Key limitation |
|----------|--------|----------------|
| SME Corp MyBPI portal | https://smecorp.gov.my | Organised by programme. No personalised matching or natural-language Q&A. |
| Singapore GoBusiness grant finder | https://www.gobusiness.gov.sg | Fixed questionnaires, no free-text input or source citations, Singapore only. |
| UK Business finance support finder | https://www.gov.uk/business-finance-support | Filter-based, with no explanation of why a scheme matches. Not Malaysian. |

## 5. Engineering Gap / Limitation

No existing tool answers free-text eligibility questions for Malaysian programmes while citing source passages and flagging uncertainty. The challenges are heterogeneous documents (HTML and PDF), fast-changing programmes (staleness), LLM hallucination of amounts and deadlines, and distinguishing grants from loans.

## 6. Project Objectives

- **O1:** To build a RAG pipeline over at least 3 official Malaysian funding sources that answers eligibility questions with passage-level citations.
- **O2:** To extract key programme attributes (type, amount, eligibility, deadline) so grants and loans are clearly distinguished.
- **O3:** To evaluate the system on a labelled set of 25-30 questions for answer correctness, citation accuracy, and abstention behaviour.

## 7. Target Users

- Student entrepreneurs and early-stage founders
- Owners of micro, small, and medium enterprises
- University incubator and entrepreneurship office staff

## 8. Data Sources

| Source | URL |
|--------|-----|
| SME Corp Malaysia (programmes, financing, PDFs) | https://smecorp.gov.my |
| MDEC (digital economy and startup programmes) | https://mdec.my |
| Cradle Fund (startup funding) | https://www.cradle.com.my |

## 9. Comparison with EduAdvisor

**Similar:** Both use RAG over public documents, answer natural-language questions with citations, serve a defined user group, and are evaluated on a labelled question set.

**Different:** GrantGuide works in funding rather than university information. It must handle structured eligibility rules (turnover, headcount, ownership), so it combines structured extraction with RAG. It also faces staleness because programmes open and close, and it must separate grants from loans, which has financial consequences.

## 10. Responsible AI Risks

- **Hallucinated or outdated details** (amounts, deadlines, eligibility). *Mitigation:* require citations, show source dates, abstain when retrieval confidence is low.
- **Loans mistaken for grants, or implied approval.** *Mitigation:* label every result with programme type and use "may be eligible" wording.
- **Language and regional bias** (English-only coverage). *Mitigation:* include Bahasa Malaysia sources and test across regions.
