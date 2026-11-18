# GrantGuide: Week 3 Pipeline Decisions

## 1. Requirements and Source

**Topic 2 requirements addressed:**
- **FR-9:** the system shall provide a re-runnable ingestion pipeline that records source URL and retrieval date.
- **FR-5 / FR-6:** citations and source dates need each chunk to be traceable to its source.
- **RAI-5 / NFR-3:** freshness transparency and reproducibility.

**Selected source:** `msme_tnc (1).pdf`, the BSN MSME Digital Grant MADANI Terms and Conditions (S01 in `week3_data_inventory.md`).

**Why one source:** V1 processes one representative grant document to validate the pipeline end to end. The FAQ (S02) was inspected to confirm a second document structure and will be added in later weeks.

**Pathway:** A (PDF/TXT).

## 2. Part B Comparison: Chunking Approaches

Both approaches were run on the same cleaned text with a chunk size of 500 characters.

| Approach | Chunks | Observed evidence | Notes |
|----------|--------|-------------------|-------|
| Fixed-size (500 characters) | 22 | 10,791 cleaned characters split into 22 chunks of 500 characters (the last is shorter). Boundaries fall at fixed positions regardless of content. Boundary example: chunk 1 ends with "...to assist PMKS in utilizing digital ser" and chunk 2 starts with "vices in their daily business operations...", so the word "services" is split across two chunks. | Simple, but can split an eligibility condition across chunks. |
| Paragraph-aware (500 characters) | 29 | 29 chunks, so about 370 characters on average (10,791 / 29). The first chunk is a short heading fragment (the title plus the page number "1", roughly 105 characters), which shows that headings can become very small chunks. | Keeps paragraph boundaries, so related conditions stay together, but produces more and uneven chunks. |

**Selected approach:** paragraph-aware.

**Justification:** Paragraph-aware chunking produced 29 chunks against 22 for fixed-size, because it splits on the document's own paragraph breaks instead of at every 500 characters. For FR-5 (citations), a chunk that holds one complete condition is easier to cite correctly than a chunk that cuts a rule in half. The fixed-size boundary example confirms the risk: it cut the word "services" in half at the end of the first chunk, and a cut like this inside an eligibility condition would damage both retrieval and citation.

**Benefit:** chunk boundaries follow the document's paragraph structure.
**Accepted limitation:** some chunks are very short (for example the heading chunk), and chunk sizes are uneven. Short fragments are flagged in the Topic 4 audit. Page numbers are also not kept.

## 3. Chosen Preparation Configuration

| Setting | Value |
|---------|-------|
| Library | pypdf |
| Cleaning | Collapse repeated spaces and tabs; strip leading and trailing whitespace |
| Chunking | Paragraph-aware |
| Chunk size | 500 characters |
| Output format | JSONL, one record per chunk |
| Record fields | record_id, text, source, source_sha256, processing, page |

## 4. Accepted Limitations

- **Page numbers are not kept** (`page` is `None`), because the simple splitter works on the whole document text. Page-level citation is a later improvement.
- **Source URL and retrieval date are not stored per chunk.** They are recorded in the inventory only. Adding them to each chunk is needed for FR-5 and FR-6 in a later iteration.
- **Tables are flattened by text extraction.** The extracted text begins with column headings ("Bil ITEM Information") followed by numbered items run together, which shows the document's table layout is converted into running text.
- **Scanned PDFs** would need OCR, which is outside V1.

## 5. ETL vs ELT Decision

**Decision: ETL.** The document is extracted, cleaned, and chunked before it is stored as prepared data.

**Rationale:** The corpus is small, and the unprepared PDF text is not useful to the downstream retriever. Cleaning before storage keeps the prepared file small and consistent. The raw PDFs are retained unchanged, so the process can be rerun with a different configuration.

## 6. Batch vs Streaming Decision

**Decision: batch.** The pipeline runs on demand over static documents.

**Rationale:** Grant documents change only when a programme round is updated, not continuously. A manual or scheduled batch run is enough. Streaming would add complexity with no benefit at this stage.

## 7. Stage Contracts

| Stage | Contract / evidence | Failure response |
|-------|---------------------|------------------|
| Ingest | Chosen source file exists and is permitted | Stop for missing input |
| Inspect | PDF is readable and contains extractable text | Stop and flag possible scanned PDF |
| Prepare | Chunking produces at least one non-empty chunk | Stop and report the transformation failure |
| Validate | Output file exists, is non-empty, and has a SHA-256 fingerprint | Stop for missing or empty output |
| Record lineage | Source links to the exact output, with checksums | Stop and report missing mapping or metadata |

## 8. Output Locations

| Artefact | Location |
|----------|----------|
| Part B prepared output | `data/processed/prepared_text.jsonl` |
| Part B lineage manifest | `data/week3_manifest.json` |
| Part C pipeline output | `data/pipeline_v1/documents/prepared_text.jsonl` |
| Part C run manifest | `data/pipeline_v1/documents/pipeline_manifest.json` |
| Notebook | `notebooks/week3_pipeline.ipynb` |

## 9. Repeatability Evidence

Cell 9 reran the pipeline and compared source fingerprints, output fingerprints, and source-to-output mapping. Result: **PASS** for the documents pipeline. The rerun produced identical source fingerprints, prepared-output fingerprints, and source-to-output mapping (1 input, 1 output). This is a repeatability smoke test, not full cross-environment reproducibility.

## 10. Provenance and Access Caveat

The source is a public document, but `source_version` and `access_rights` are recorded as "Not verified" in the manifest. The document date and terms of use are noted in the inventory. The SHA-256 checksum proves the file has not changed, not that the content is current or authorised for redistribution. The raw PDFs are not committed to GitHub unless their terms allow it. [FILL IN: your decision].

## 11. Traceability Update

| Requirement | Pipeline element | Evidence |
|-------------|------------------|----------|
| FR-9 re-runnable ingestion | Cells 7, 8A, 9 | Pipeline manifest and PASS repeatability check |
| FR-5 citations | `source` and `source_sha256` per chunk | `prepared_text.jsonl` |
| FR-6 source dates | Retrieval date in inventory (not yet per chunk) | `week3_data_inventory.md` |
| NFR-3 reproducibility | Same config reproduces identical outputs | Cell 9 result |
| RAI-5 freshness transparency | Document date recorded; per-chunk date is a later improvement | `week3_data_inventory.md` |
