# Regulatory Maintenance & Knowledge Workflows

Given the highly regulated nature of the banking sector, the Agentic AI's knowledge base cannot rely on ad-hoc updates. It must adhere to a deterministic pipeline for integrating new regulatory guidelines (e.g., RBI circulars) and an audit-heavy modification workflow.

## The RBI Update Pipeline

When the Reserve Bank of India (RBI) issues a fresh circular (e.g., changes to repo rates, new digital lending guidelines, or updated KYC requirements), the KB must be updated deterministically to ensure the Agent stops dispensing outdated advice immediately.

### Workflow
1. **Document Ingestion**: The new RBI circular (PDF/Text) is uploaded to the Knowledge Ingestion Tool.
2. **Metadata Tagging**: The document is mapped to the `kb_schema.md` with:
   - `compliance_tags`: e.g., `rbi_circular_ref: RBI/2026-27/045`
   - `publication_date`: Effective date of the regulation.
3. **Deprecation Check**: The system automatically queries the Vector DB for existing chunks sharing the exact intent/category and flags them as `Pending Deprecation`.
4. **Chunking & Embedding**: The new document is chunked and embedded.
5. **Supervisor Sign-off**: (See Audit Logging Spec below).

## Audit Logging & Supervisor Sign-Off

To maintain strict compliance and prevent hallucinations due to bad data ingestion, **no schema or knowledge chunk modifications can go live in the production vector store without Human-in-the-Loop (HITL) approval.**

### Staging vs. Production Stores
- **Staging Store**: A sandbox vector store where the updated/new chunks are initially committed.
- **Production Store**: The live database queried by the Agent.

### The Sign-Off Protocol
1. **Diff Generation**: The ingestion system generates a "Knowledge Diff", showing:
   - Newly added chunks.
   - Chunks marked for deprecation/deletion.
   - The impact analysis (which intents/queries this might affect).
2. **Automated Testing**: A suite of standard regression questions is run against the Staging Store to verify that the new information is retrieved correctly.
3. **Supervisor Approval**: A human compliance officer reviews the Knowledge Diff and test results in a dashboard.
4. **Atomic Commit**: Upon approval, the `Production Store` is updated atomically (a transaction replacing the old chunks with the new ones).
5. **Audit Log Record**: A permanent, immutable record is written to the audit database containing:
   - `timestamp`: Time of commit.
   - `supervisor_id`: ID of the human approver.
   - `action`: `UPSERT`/`DELETE`.
   - `document_id`: The source document modified.
   - `diff_hash`: Cryptographic hash of the content change.

This guarantees that every piece of advice the Agent dispenses can be traced back to an explicit human approval of the source data.
