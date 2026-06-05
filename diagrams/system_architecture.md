# NexBank Agentic AI — System Architecture Diagrams

## 1. End-to-End System Topology (Architecture)

```mermaid
graph TB
    %% STYLING
    classDef client fill:#2d3436,stroke:#636e72,color:#fff,rx:5px,ry:5px
    classDef gateway fill:#1a1a2e,stroke:#e94560,color:#fff,stroke-width:2px
    classDef guardrail fill:#c0392b,stroke:#e74c3c,color:#fff,stroke-width:2px,stroke-dasharray: 5 5
    classDef nlu fill:#0f3460,stroke:#533483,color:#fff
    classDef core fill:#533483,stroke:#9b59b6,color:#fff,stroke-width:2px
    classDef rag fill:#27ae60,stroke:#2ecc71,color:#fff
    classDef llm fill:#d35400,stroke:#e67e22,color:#fff
    classDef audit fill:#7f8c8d,stroke:#95a5a6,color:#fff
    
    %% NODES & SUBGRAPHS
    subgraph CLIENT_EDGE ["Client Edge"]
        APP(Mobile App) ::: client
        WEB(Web Chat) ::: client
        WA(WhatsApp) ::: client
    end

    subgraph INGRESS_LAYER ["Ingress & Pre-Flight Security"]
        GW["API Gateway (C1)<br/>Rate Limits & TLS 1.3"] ::: gateway
        PRE_GR["Pre-Flight Guardrail (C6a)<br/>PII Ingestion Gate & Base64 Scrubber"] ::: guardrail
    end

    subgraph ORCHESTRATION ["Core Orchestration"]
        DME{"Dialogue Management<br/>Engine (C3)"} ::: core
    end

    subgraph PARALLEL_NLU ["Parallel NLU Processing (C2)"]
        direction LR
        INTENT["Intent Classifier<br/>(37 Domains)"] ::: nlu
        ENTITY["Entity Extractor<br/>(15 Slot Types)"] ::: nlu
        SENTIMENT["Sentiment Analyzer<br/>(Threshold: -0.7)"] ::: nlu
    end

    subgraph RAG_PIPELINE ["Hybrid RAG Pipeline (C4)"]
        direction LR
        SPARSE[(BM25 Sparse)] ::: rag
        DENSE[(Vector DB Dense)] ::: rag
        FUSION["Reciprocal Rank Fusion<br/>(RRF k=60)"] ::: rag
    end

    subgraph GENERATION_EGRESS ["Generation & Post-Flight"]
        INFERENCE["LLM Inference Engine (C5)<br/>Temperature = 0.3"] ::: llm
        POST_GR["Post-Flight Validator (C6b)<br/>Semantic Output Scanner"] ::: guardrail
    end

    subgraph ESCALATION_AUDIT ["Safety & Compliance"]
        ESC["Escalation Router (C7)<br/>(15 Triggers, SLAs)"] ::: guardrail
        AUDIT[("Encrypted Audit Ledger (C9)<br/>AES-256 / WORM Storage")] ::: audit
    end

    %% RELATIONSHIPS & FLOW
    CLIENT_EDGE -->|"Raw Payload (JSON)"| GW
    GW -->|"Normalized Payload"| PRE_GR
    
    PRE_GR -- "Sanitized Text" --> DME
    PRE_GR -. "PII Scrub Event" .-> AUDIT
    
    DME -->|"Async Dispatch"| PARALLEL_NLU
    INTENT & ENTITY & SENTIMENT -->|"Aggregated Context"| DME
    
    DME -->|"Query Condition Meets 'KB_Required'"| RAG_PIPELINE
    RAG_PIPELINE -->|"Top-3 Chunks (Score >= τ)"| DME
    
    DME -->|"Structured Context + History"| INFERENCE
    INFERENCE -->|"Raw LLM Completion"| POST_GR
    
    POST_GR -->|"Clean Response (Latency < 2.5s)"| GW
    POST_GR -. "Financial Advice Detected!" .-> ESC
    
    DME -. "Sentiment < -0.7" .-> ESC
    
    GW & DME & INFERENCE & ESC -. "Async Event Stream (Kafka)" .-> AUDIT
```

---

## 2. Request Processing Flow (Happy Path)

```mermaid
sequenceDiagram
    participant C as 📱 Customer
    participant GW as C1: Gateway
    participant GR as C6: Guardrails
    participant NLU as C2: NLU Pipeline
    participant DME as C3: Dialogue Engine
    participant KB as C4: Knowledge Base
    participant RG as C5: Response Gen
    participant AL as C9: Audit Log

    C->>GW: "What is my savings account balance?"
    activate GW
    Note over GW: Session lookup, language detection (50ms)
    GW->>GR: Input validation
    activate GR
    Note over GR: Prompt injection scan, PII check (50ms)
    GR-->>GW: ✅ Input CLEAN
    deactivate GR

    GW->>NLU: Classify message
    activate NLU
    Note over NLU: Intent: account.balance_check (0.91)<br/>Entity: account_type=savings<br/>Sentiment: neutral (0.1)<br/>Latency: 200ms
    NLU-->>DME: Classification result
    deactivate NLU

    activate DME
    Note over DME: State: S3→S6 (all slots filled)<br/>Auth: OTP verified ✅<br/>Policy: RETRIEVE_AND_RESPOND
    DME->>KB: Hybrid retrieval query
    activate KB
    Note over KB: Dense + BM25 + RRF fusion<br/>Re-rank top 5 candidates<br/>Latency: 100ms
    KB-->>DME: Knowledge results (confidence: 0.93)
    deactivate KB

    DME->>RG: Generate response
    activate RG
    Note over RG: Method: TEMPLATE (safety-critical)<br/>Template: TMPL-ACC-BAL-001<br/>Latency: 210ms
    RG-->>DME: Generated response
    deactivate RG

    DME->>GR: Validate response
    activate GR
    Note over GR: ✅ PII masked<br/>✅ No financial advice<br/>✅ Grounded in KB<br/>✅ Tone OK<br/>Latency: 45ms
    GR-->>DME: ✅ All checks PASS
    deactivate GR

    DME-->>GW: Final response
    deactivate DME
    GW-->>C: "Your NexBank Savings Account ending in 1234 has a current available balance of ₹45,230.50."
    deactivate GW

    Note over AL: All events logged async<br/>Correlation ID: session+msg

    par Async Logging
        GW--)AL: Gateway events
        NLU--)AL: NLU events
        DME--)AL: State transitions
        KB--)AL: Retrieval events
        RG--)AL: Generation events
        GR--)AL: Guardrail results
    end
```

---

## 3. Complex Dialogue State Machine (State Management)

```mermaid
stateDiagram-v2
    %% STATE STYLES
    classDef initial fill:#2d3436,stroke:#636e72
    classDef auth fill:#f39c12,stroke:#e67e22,color:#fff
    classDef active fill:#2980b9,stroke:#3498db,color:#fff
    classDef block fill:#c0392b,stroke:#e74c3c,color:#fff
    classDef endstate fill:#27ae60,stroke:#2ecc71,color:#fff

    [*] --> Idle : Connect
    
    state "Idle (Session Initiated)" as Idle ::: initial
    state "Authenticated_Check" as AuthCheck ::: auth
    state "Intent_Processing (NLU)" as IntentProcessing ::: active
    state "Slot_Filling_Active" as SlotFilling ::: active
    state "Context_Carryover" as ContextCarry ::: active
    state "KB_Query (RAG)" as KBQuery ::: active
    state "Guardrail_Triggered" as Guardrail ::: block
    state "Human_Escalation_Queue" as Escalation ::: block
    state "Session_Teardown" as Teardown ::: endstate

    Idle --> AuthCheck : Input Detected [Timeout = 5m]
    
    AuthCheck --> ContextCarry : [Valid Auth Token & Recent History]
    AuthCheck --> IntentProcessing : [Auth Level Matches Required]
    AuthCheck --> Guardrail : [Auth Level Insufficient for Action]

    ContextCarry --> IntentProcessing : Merge History

    IntentProcessing --> SlotFilling : [Entities Missing (Confidence > 0.85)]
    IntentProcessing --> KBQuery : [All Entities Gathered]
    IntentProcessing --> Guardrail : [NLU Detects Banned Topic]
    IntentProcessing --> Escalation : [TurnCounter > 3 w/ Low Confidence]

    SlotFilling --> IntentProcessing : Entity Provided
    SlotFilling --> Escalation : [Failed to Fill Slot > 3x]

    KBQuery --> Guardrail : [RRF Score < τ_rerank]
    KBQuery --> IntentProcessing : [KB Retrieved Successfully]

    Guardrail --> IntentProcessing : [Sanitized / Handled via Template]
    Guardrail --> Escalation : [Critical Violation / Sentiment < -0.7]

    Escalation --> Teardown : Transfer Payload to Human
    IntentProcessing --> Teardown : Task Resolved

    Teardown --> [*] : Commit to Audit Ledger
```

---

## 4. Failure & Escalation Flow

```mermaid
flowchart TD
    START["🔍 Component Failure Detected"] --> TYPE{Failure Type?}

    TYPE -->|"NLU Timeout/Crash (F01)"| NLU_FB["Fallback: Use last intent<br/>or ask to rephrase"]
    TYPE -->|"Low Confidence (F02)"| LOW_CONF["Trigger Disambiguation<br/>State → S5"]
    TYPE -->|"KB No Results (F05)"| KB_FB["Acknowledge uncertainty<br/>Soft escalate to specialist"]
    TYPE -->|"LLM Error (F06)"| LLM_FB["Fallback to template-only<br/>response"]
    TYPE -->|"Hallucination (F07)"| HALL["Block response<br/>Regenerate with strict grounding"]
    TYPE -->|"Guardrail Engine Down (F09)"| SAFE["⚠️ SAFE MODE<br/>ALL sessions → Human Agents"]
    TYPE -->|"No Agents Available (F10)"| QUEUE["Offer callback<br/>Provide case reference"]

    NLU_FB --> CHECK{Resolved?}
    LOW_CONF --> CHECK
    KB_FB --> CHECK
    LLM_FB --> CHECK
    HALL --> CHECK
    QUEUE --> CHECK

    CHECK -->|Yes| CONTINUE["✅ Continue Conversation"]
    CHECK -->|"No (3+ retries)"| ESC["🚨 Escalate to Human Agent"]

    SAFE --> RESTORE{"C6 Restored<br/>& Verified?"}
    RESTORE -->|Yes| RESUME["✅ Resume Normal Operations"]
    RESTORE -->|No| WAIT["Keep Safe Mode Active"]

    ESC --> LOG["📝 Log to Audit Service"]
    CONTINUE --> LOG
    RESUME --> LOG

    style SAFE fill:#e74c3c,stroke:#c0392b,color:#fff
    style ESC fill:#e67e22,stroke:#d35400,color:#fff
    style CONTINUE fill:#27ae60,stroke:#2ecc71,color:#fff
    style RESUME fill:#27ae60,stroke:#2ecc71,color:#fff
```

---

## 5. Latency Budget Breakdown

```mermaid
gantt
    title End-to-End Latency Budget (p95 = 3,000ms)
    dateFormat X
    axisFormat %L ms

    section Pipeline
    C1 Gateway Input (50ms)           :a1, 0, 50
    C6 Input Guardrails (50ms)        :a2, 0, 50
    C2 NLU Classification (500ms)     :a3, 50, 550
    C3 Dialogue Policy (50ms)         :a4, 550, 600
    C4 KB Retrieval (200ms)           :a5, 600, 800
    C5 Response Generation (1500ms)   :a6, 800, 2300
    C6 Output Guardrails (100ms)      :a7, 2300, 2400
    C1 Gateway Output (50ms)          :a8, 2400, 2450

    section Parallel
    Note: Input Guardrails run parallel with NLU :milestone, 0, 0

    section Async
    C9 Audit Logging (0ms on critical path) :done, a9, 0, 0
```

---

## 6. Scalability Architecture (100x)

```mermaid
graph TB
    subgraph REGION1["Region: ap-south-1 (Mumbai) — Primary"]
        subgraph AZ1["Availability Zone 1"]
            GW1["Gateway x20"]
            NLU1["NLU x20 (GPU)"]
            DME1["DME x40"]
            KB1["KB Cluster Node x5"]
        end
        subgraph AZ2["Availability Zone 2"]
            GW2["Gateway x20"]
            NLU2["NLU x20 (GPU)"]
            DME2["DME x40"]
            KB2["KB Cluster Node x5"]
        end
        LB1["Regional Load Balancer"]
        REDIS1["Redis Cluster (6 nodes)"]
        PG1["PostgreSQL Primary + 2 Replicas"]
        KAFKA1["Kafka Cluster (10 brokers)"]
    end

    subgraph REGION2["Region: ap-south-2 (Hyderabad) — DR"]
        GW3["Gateway x10"]
        NLU3["NLU x10 (GPU)"]
        DME3["DME x20"]
        KB3["KB Cluster Node x5"]
        REDIS2["Redis Replica"]
        PG2["PostgreSQL Standby"]
        KAFKA2["Kafka Mirror"]
    end

    GLB["Global Load Balancer / DNS"] --> LB1
    GLB -.->|"Failover"| REGION2

    LB1 --> AZ1
    LB1 --> AZ2

    PG1 -.->|"Async Replication"| PG2
    KAFKA1 -.->|"MirrorMaker"| KAFKA2
    REDIS1 -.->|"Cross-Region Sync"| REDIS2

    style REGION1 fill:#1a1a2e,stroke:#e94560,color:#e0e0e0
    style REGION2 fill:#16213e,stroke:#0f3460,color:#e0e0e0
    style AZ1 fill:#0f3460,stroke:#533483,color:#e0e0e0
    style AZ2 fill:#0f3460,stroke:#533483,color:#e0e0e0
```

---

*Diagrams generated for Day 2 — System Architecture Specification*
