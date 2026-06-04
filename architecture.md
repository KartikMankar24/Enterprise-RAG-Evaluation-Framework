# System Architecture

```mermaid
flowchart LR
  A["PDF Corpus"] --> B["PDF Loader"]
  B --> C["Chunking and Metadata Enrichment"]
  C --> D["Embedding Model"]
  D --> E{"Vector Database"}
  E --> E1["ChromaDB"]
  E --> E2["Pinecone"]
  E --> E3["Weaviate"]
  E --> F["Retriever Strategy Layer"]
  F --> F1["Vector Similarity"]
  F --> F2["MMR Diversity"]
  F --> F3["BM25 Keyword"]
  F --> F4["Hybrid Ensemble"]
  F --> G["RAG Generation Chain"]
  G --> H["RAGAS Evaluation"]
  H --> I["CSV and Markdown Reports"]
  I --> J["Streamlit Dashboard"]
```

## Components

- Ingestion: loads PDFs, preserves source/page metadata, and chunks text for retrieval.
- Vector stores: supports ChromaDB for local development, Pinecone for managed scale, and Weaviate for self-hosted or hybrid deployments.
- Retrieval comparison: runs the same evaluation set across similarity, MMR, BM25, and hybrid ensemble strategies.
- Generation: uses a grounded LangChain prompt that refuses unsupported answers.
- Evaluation: computes context precision, context recall, faithfulness, and answer relevancy through RAGAS.
- Reporting: emits machine-readable CSVs, an executive markdown report, and an interactive dashboard.

## Production Control Plane

```mermaid
flowchart TB
  U["Knowledge Admin"] --> API["Ingestion API"]
  API --> Q["Document Queue"]
  Q --> W["Embedding Workers"]
  W --> V["Vector DB"]
  V --> R["Retrieval Service"]
  R --> L["LLM Gateway"]
  L --> OBS["Tracing, Cost, Quality Telemetry"]
  OBS --> EVAL["Nightly Evaluation Jobs"]
  EVAL --> DASH["Quality Dashboard"]
  EVAL --> ALERT["Regression Alerts"]
```
