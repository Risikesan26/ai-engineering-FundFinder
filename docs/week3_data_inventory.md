# GrantGuide: Week 3 Data Inventory

> Replace every [FILL IN] with your real Colab results before submitting. Do not leave placeholders or invent numbers.

## Requirement-to-data mapping (Step A1)

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|----------------|------------------------------|------------------|--------------------------|
| FR-9: re-runnable ingestion pipeline | Grant eligibility rules and funding terms as text | MSME Digital Grant MADANI Terms and Conditions (BSN, PDF) | Official, public document from the administering bank. Text-based PDF. Covers eligibility conditions. |
| FR-5 / FR-6: citation and source date | Source identity, URL, and document date for each passage | Same document plus recorded URL and download date | Source filename and SHA-256 are stored per chunk. URL and retrieval date are recorded here and are not yet stored per chunk. |
| RAI-5 / NFR-3: freshness and reproducibility | Version and date of each source, and repeatable processing | Same document | Programme rules change by round, so version and date must be recorded. |

## Data inventory (Step A4)

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|-----------|----------------|------------------|------------------------|-----------------|---------------------|
| S01 | `msme_tnc (1).pdf`; https://www.bsn.com.my/cms/upload/pdf/business/msme_tnc.pdf | PDF, grant Terms and Conditions. Processed in the V1 pipeline. | Owner: Bank Simpanan Nasional (BSN) with MDEC and MCMC. Document date: [FILL IN]. Downloaded: [FILL IN date]. | Publicly accessible on the BSN website. Used for educational, non-commercial purposes. Terms of use checked: [FILL IN]. | Pages: 7. Extracted characters: 10,799 (10,791 after whitespace cleaning). Sample text: "TERMS AND CONDITIONS FOR MADANI'S MICRO, SMALL, AND MEDIUM ENTERPRISES (PMKS) DIGITAL GRANT (GDPM) ... Bil ITEM Information 1 Introduction a) The Madani Micro, Small, and Medium Enterprises (PMKS) Digital Grant is an initiative grant provided by the Malaysian Ministry of Finance...". Observed issue: text is readable, but the table layout (Bil / ITEM / Information columns) is flattened into running text with stray line breaks. |
| S02 | `msme_faq (1).pdf`; https://www.bsn.com.my/cms/upload/pdf/business/msme_faq.pdf | PDF, grant FAQ. Inspected only, not processed in the V1 pipeline. | Owner: BSN. Document date: [FILL IN]. Downloaded: [FILL IN date]. | Publicly accessible. Used for educational, non-commercial purposes. | Pages: [FILL IN]. Extracted characters: [FILL IN]. Sample text: [FILL IN]. Structure: Q&A format. |
| S03 (optional) | `CIP Spark - Cradle F...` ; https://cradle.com.my/grants/cip-spark/ | Saved web page, grant programme description. [Inspected only / not inspected: FILL IN] | Owner: Cradle Fund. Downloaded: [FILL IN date]. | Publicly accessible. | [FILL IN or delete this row if not inspected]. |

## Storage

- **Raw files:** `data/raw/` in the Colab workspace (temporary). Original PDFs are not modified.
- **Prepared output:** `data/processed/prepared_text.jsonl`.
- **Lineage:** `data/week3_manifest.json`.
- **Pipeline run:** `data/pipeline_v1/documents/`.

## Notes

- S02 and S03 were inspected to confirm that other document structures can be read. They are not part of the V1 pipeline run. They will be added in later weeks.
- All sources are public documents, and no personal data is expected. This is checked formally in the Topic 4 audit.
- Source version and access rights are recorded as "Not verified" in the manifest and are documented in this table.
