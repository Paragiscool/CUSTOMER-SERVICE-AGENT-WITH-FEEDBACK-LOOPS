# Hybrid Retrieval Architecture

To meet the strict **<200ms latency budget ($P_{95}$)** while maintaining extremely high precision for financial queries, the NexBank Knowledge Base utilizes a two-stage hybrid retrieval pipeline.

## System Architecture

```mermaid
flowchart TD
    %% STYLING
    classDef input fill:#2c3e50,stroke:#34495e,color:#fff
    classDef db fill:#2980b9,stroke:#3498db,color:#fff,shape:cylinder
    classDef compute fill:#f39c12,stroke:#e67e22,color:#fff
    classDef math fill:#8e44ad,stroke:#9b59b6,color:#fff
    classDef decision fill:#c0392b,stroke:#e74c3c,color:#fff,shape:diamond
    classDef output fill:#27ae60,stroke:#2ecc71,color:#fff
    
    Q["Query: 'What are the NRI FD Rates?'"] ::: input
    
    subgraph PARALLEL_RETRIEVAL ["Stage 1: Parallel Hybrid Retrieval"]
        direction LR
        BM25_IDX[(Sparse Index<br/>BM25)] ::: db
        VEC_IDX[(Dense Index<br/>Vector Embeddings)] ::: db
        
        BM25_SCORE["BM25 Scoring<br/>(Keyword Exact Match)"] ::: compute
        COSINE_SCORE["Cosine Similarity<br/>(Semantic Match)"] ::: compute
        
        BM25_IDX --> BM25_SCORE
        VEC_IDX --> COSINE_SCORE
    end
    
    Q --> PARALLEL_RETRIEVAL
    
    subgraph FUSION_ENGINE ["Stage 2: Score Normalization & Fusion"]
        NORM["Score Normalization<br/>(Min-Max Scaling)"] ::: compute
        RRF["Reciprocal Rank Fusion<br/>RRF_Score = 1 / (k + rank)"] ::: math
        K_CONST["Smoothing Constant<br/>(k = 60)"] ::: math
    end
    
    BM25_SCORE --> NORM
    COSINE_SCORE --> NORM
    NORM --> RRF
    K_CONST -.-> RRF
    
    RRF -->|"Top-20 Candidates"| CROSS_ENC["Cross-Encoder Re-Ranker Model<br/>(e.g., ms-marco-MiniLM)"] ::: compute
    
    subgraph THRESHOLD_GATE ["Stage 3: Mathematical Threshold Gate"]
        CHECK{"Score >= τ ?<br/>(τ = 0.75)"} ::: decision
        FAIL["Fail-shut<br/>Trigger Escalation"] ::: output
        PASS["Top-3 Context Chunks"] ::: output
    end
    
    CROSS_ENC --> CHECK
    CHECK -- "No (Low Confidence)" --> FAIL
    CHECK -- "Yes (High Confidence)" --> PASS
    
    PASS -->|"Inject into Context"| LLM["LLM Window"] ::: input
```

## Stage 1: Retrieval Fusion

Stage 1 runs in parallel to minimize latency.
- **Sparse Retrieval**: Uses BM25 to capture exact keyword matches (e.g., specific product codes like `NEX-CC-PREM`, explicit account terms, numerical limits).
- **Dense Retrieval**: Uses an embedding model (e.g., `bge-m3`) to capture semantic meaning and intent.

### Mathematical Framework (RRF)
To guarantee high structural precision, the pipeline merges the results using Reciprocal Rank Fusion ($RRF$). The score for a document $d \in D$ is calculated as:

$$RRF\_Score(d) = \sum_{m \in M} \frac{1}{k + r_m(d)}$$

Where:
* $M$ represents the set of retrieval strategies (Sparse BM25 and Dense Vector embeddings).
* $r_m(d)$ is the ordinal rank of document $d$ within the output of retrieval system $m$.
* $k$ is a smoothing constant set to $60$ to mitigate the disproportionate impact of low-ranked outliers.

## Stage 2: Cross-Encoder Re-ranking

The top 15–20 results from Stage 1 are passed to a lightweight Cross-Encoder model (e.g., `ms-marco-MiniLM`).
Unlike bi-encoders which process the query and document separately, the cross-encoder processes `(Query, Document)` pairs together, allowing cross-attention between terms to output a highly accurate final relevance score.

Only the **Top 3–5 chunks** are ultimately passed to the LLM's context window.

## Confidence Scoring & Uncertainty Thresholds

To prevent hallucinations, we enforce an explicit mathematical boundary on the re-ranker's output.

1. **Threshold Definition**: Let $\tau_{re-rank} = 0.75$ (configurable based on empirical A/B testing).
2. **Condition**: If the maximum score of the re-ranked documents $S_{max} < \tau_{re-rank}$:
   - The system **MUST NOT** pass the context to the LLM.
   - The generation step is bypassed.
   - An instant fallback/escalation routine is triggered (e.g., "I don't have the exact information on hand, let me transfer you to an agent.").

This guarantees the agent never guesses numerical rates or unverified policies.
