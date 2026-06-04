# NexBank Agentic AI — System Architecture Diagrams

## 1. High-Level System Architecture

```mermaid
graph TB
    subgraph CHANNELS["📱 Customer Channels"]
        MA["Mobile App"]
        WC["Web Chat"]
        WA["WhatsApp Business"]
        IVR["IVR / Voice"]
    end

    subgraph LAYER1["LAYER 1: Channel & Gateway"]
        GW["C1: User Interface Gateway<br/>━━━━━━━━━━━━━━━━━━━<br/>• Channel Normalisation<br/>• Session Management<br/>• Rate Limiting & DDoS<br/>• TLS Termination<br/>• Language Detection"]
    end

    subgraph LAYER2["LAYER 2: Understanding"]
        NLU["C2: NLU Pipeline<br/>━━━━━━━━━━━━━━━━━━━<br/>• 3-Tier Intent Classification<br/>• Entity Extraction & Validation<br/>• Sentiment Analysis<br/>• Multi-Intent Detection<br/>• Out-of-Scope Detection"]
        DME["C3: Dialogue Management Engine<br/>━━━━━━━━━━━━━━━━━━━<br/>• State Machine (16 states)<br/>• Slot-Filling Orchestration<br/>• Policy Decision (Hybrid)<br/>• Multi-Turn Reasoning<br/>• Context Window Mgmt"]
    end

    subgraph LAYER3["LAYER 3: Action & Generation"]
        KB["C4: Knowledge Base & Retrieval<br/>━━━━━━━━━━━━━━━━━━━<br/>• Hybrid Retrieval (Dense+Sparse)<br/>• Cross-Encoder Re-Ranking<br/>• Access Control Matrix<br/>• Version Control & TTL<br/>• p95 < 200ms"]
        RG["C5: Response Generation<br/>━━━━━━━━━━━━━━━━━━━<br/>• Template-Based (Safety-Critical)<br/>• LLM-Based (Conversational)<br/>• Hybrid Blending Layer<br/>• Multi-Language Support<br/>• Persona Enforcement"]
        GR["C6: Guardrail & Safety Engine<br/>━━━━━━━━━━━━━━━━━━━<br/>• Input Guardrails (Injection, PII)<br/>• Processing Guardrails (Auth, Advice)<br/>• Output Guardrails (Hallucination)<br/>• IMMUTABLE Safety Layer<br/>• 8 Mandatory Security Rules"]
        ER["C7: Escalation Router<br/>━━━━━━━━━━━━━━━━━━━<br/>• 15 Trigger Conditions<br/>• Priority Classification (P0-P3)<br/>• 8-Element Handoff Protocol<br/>• Queue Management<br/>• De-Escalation Logic"]
    end

    subgraph LAYER4["LAYER 4: Platform & Intelligence"]
        CL["C8: Continuous Learning Pipeline<br/>━━━━━━━━━━━━━━━━━━━<br/>• Supervisor Corrections<br/>• CSAT Signal Integration<br/>• Resolution Outcome Tracking<br/>• Safety-Preserving Updates<br/>• A/B Testing Framework"]
        AL["C9: Audit & Logging Service<br/>━━━━━━━━━━━━━━━━━━━<br/>• Immutable Event Log<br/>• Compliance Metadata<br/>• 7-Year Retention (Financial)<br/>• Tamper-Proof Storage (WORM)<br/>• Real-Time Dashboards"]
    end

    subgraph AGENTS["👤 Human Agent Teams"]
        FI["Fraud Investigation"]
        SR["Senior Resolution Mgr"]
        CR["Customer Retention"]
        GS["General Support"]
    end

    subgraph DATA["💾 Data Layer"]
        REDIS["Redis Cluster<br/>Session State & Cache"]
        PG["PostgreSQL<br/>Conversation History"]
        VDB["Qdrant / Weaviate<br/>Vector Embeddings"]
        ES["Elasticsearch<br/>Sparse Retrieval & Logs"]
        KAFKA["Apache Kafka<br/>Event Streaming"]
        S3["Object Storage (WORM)<br/>Audit Archival"]
    end

    MA --> GW
    WC --> GW
    WA --> GW
    IVR --> GW

    GW -->|"Normalised Message"| GR
    GR -->|"Sanitised Input"| NLU
    NLU -->|"Intent + Entities + Sentiment"| DME
    DME -->|"Retrieval Query"| KB
    KB -->|"Knowledge Results"| DME
    DME -->|"Generation Request"| RG
    RG -->|"Generated Response"| GR
    GR -->|"Validated Response"| GW
    DME -->|"Escalation Trigger"| ER
    ER --> FI
    ER --> SR
    ER --> CR
    ER --> GS

    DME --> REDIS
    DME --> PG
    KB --> VDB
    KB --> ES
    AL --> KAFKA
    AL --> S3
    CL --> KAFKA

    NLU -.->|"Log"| AL
    DME -.->|"Log"| AL
    KB -.->|"Log"| AL
    RG -.->|"Log"| AL
    GR -.->|"Log"| AL
    ER -.->|"Log"| AL

    CL -.->|"Model Updates"| NLU
    CL -.->|"Template Updates"| RG
    CL -.->|"Rule Updates (non-safety)"| GR

    style LAYER1 fill:#1a1a2e,stroke:#16213e,color:#e0e0e0
    style LAYER2 fill:#16213e,stroke:#0f3460,color:#e0e0e0
    style LAYER3 fill:#0f3460,stroke:#533483,color:#e0e0e0
    style LAYER4 fill:#533483,stroke:#e94560,color:#e0e0e0
    style DATA fill:#0d1117,stroke:#30363d,color:#e0e0e0
    style CHANNELS fill:#2d3436,stroke:#636e72,color:#e0e0e0
    style AGENTS fill:#2d3436,stroke:#636e72,color:#e0e0e0
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

## 3. Dialogue State Machine

```mermaid
stateDiagram-v2
    [*] --> S0_INIT: Customer connects

    S0_INIT --> S1_GREETING: Session created

    S1_GREETING --> S3_INTENT_CLASSIFICATION: First message received
    S1_GREETING --> S14_SESSION_TIMEOUT: 5 min inactivity

    S3_INTENT_CLASSIFICATION --> S2_AUTHENTICATING: Auth upgrade needed
    S3_INTENT_CLASSIFICATION --> S4_SLOT_FILLING: Missing required entities
    S3_INTENT_CLASSIFICATION --> S5_DISAMBIGUATION: Ambiguous intent
    S3_INTENT_CLASSIFICATION --> S6_KNOWLEDGE_RETRIEVAL: Intent clear, slots filled
    S3_INTENT_CLASSIFICATION --> S10_ESCALATION: Low confidence 3+ turns
    S3_INTENT_CLASSIFICATION --> S15_ERROR_RECOVERY: NLU service error

    S2_AUTHENTICATING --> S3_INTENT_CLASSIFICATION: Auth success
    S2_AUTHENTICATING --> S10_ESCALATION: Auth failed 3x

    S4_SLOT_FILLING --> S6_KNOWLEDGE_RETRIEVAL: All slots filled
    S4_SLOT_FILLING --> S9_AWAITING_CUSTOMER: Slot prompt sent
    S4_SLOT_FILLING --> S10_ESCALATION: 3 failed attempts

    S5_DISAMBIGUATION --> S3_INTENT_CLASSIFICATION: Clarification received
    S5_DISAMBIGUATION --> S10_ESCALATION: 2 failed attempts

    S6_KNOWLEDGE_RETRIEVAL --> S7_RESPONSE_GENERATION: Knowledge retrieved
    S6_KNOWLEDGE_RETRIEVAL --> S15_ERROR_RECOVERY: KB service error

    S7_RESPONSE_GENERATION --> S8_GUARDRAIL_CHECK: Response generated

    S8_GUARDRAIL_CHECK --> S9_AWAITING_CUSTOMER: All checks PASS
    S8_GUARDRAIL_CHECK --> S7_RESPONSE_GENERATION: Non-critical fail (retry)
    S8_GUARDRAIL_CHECK --> S10_ESCALATION: Critical safety violation

    S9_AWAITING_CUSTOMER --> S3_INTENT_CLASSIFICATION: New message
    S9_AWAITING_CUSTOMER --> S14_SESSION_TIMEOUT: 5 min inactivity
    S9_AWAITING_CUSTOMER --> S12_FEEDBACK_COLLECTION: Issue resolved

    S10_ESCALATION --> S11_HUMAN_HANDOFF: Handoff prepared
    S10_ESCALATION --> S15_ERROR_RECOVERY: No agents available

    S11_HUMAN_HANDOFF --> S13_SESSION_CLOSED: Agent closes conversation

    S12_FEEDBACK_COLLECTION --> S13_SESSION_CLOSED: Feedback collected/skipped

    S14_SESSION_TIMEOUT --> S2_AUTHENTICATING: Customer returns
    S14_SESSION_TIMEOUT --> S13_SESSION_CLOSED: No return in 24h

    S15_ERROR_RECOVERY --> S9_AWAITING_CUSTOMER: Fallback sent
    S15_ERROR_RECOVERY --> S10_ESCALATION: Error persists

    S13_SESSION_CLOSED --> [*]: Audit log finalised
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
