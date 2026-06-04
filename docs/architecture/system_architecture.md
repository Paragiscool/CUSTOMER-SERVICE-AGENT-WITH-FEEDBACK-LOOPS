# System Architecture Specification

## 1. High-Level System Architecture

The NexBank Agentic AI Customer Service system follows a modular, distributed microservices architecture designed to meet high availability, sub-3s latency, and stringent financial regulatory requirements. The system is composed of 9 core components:

1. **User Interface Gateway**: The omni-channel entry point (Web, App, WhatsApp) handling authentication, session management, and rate-limiting.
2. **Dialogue Management Engine**: The central orchestrator that tracks conversation state, manages multi-turn context, handles anaphora resolution, and applies the dialogue policy.
3. **NLU Pipeline (Natural Language Understanding)**: A hierarchical intent classification and entity extraction service capable of multi-intent detection, out-of-scope identification, and sentiment analysis.
4. **Knowledge Base & Retrieval (RAG)**: A high-performance retrieval-augmented generation engine supporting dense and sparse retrieval across product docs, FAQs, and regulatory policies.
5. **Guardrail & Safety Layer**: An immutable security perimeter checking inputs (prompt injection, PII) and outputs (financial advice, hallucination) against strict compliance matrices.
6. **Response Generation Engine**: A hybrid generation module using templates for safety-critical data and LLMs for conversational empathy, complete with multi-lingual support.
7. **Escalation & Routing Manager**: Evaluates 15 trigger conditions to gracefully hand off conversations to human agents based on SLA, priority, and queue status.
8. **Continuous Learning Pipeline**: Asynchronously collects CSAT, resolution outcomes, and supervisor corrections to update models through a safety-invariant testing protocol.
9. **Audit & Compliance Logging Service**: A tamper-proof logging facility that records all interactions, classifications, and safety checks with double-encrypted PII for regulatory audits.

## 2. Dialogue State Machine Specification

The Dialogue Management Engine operates on a deterministic state machine overlaid with probabilistic intent transitions.

### Core States (16 Total: S0–S15)
- `S0_INIT`: Session started, awaiting channel handshake.
- `S1_GREETING`: Welcome message sent, awaiting first customer input.
- `S2_AUTHENTICATING`: Verifying user identity (OTP, Biometric, KYC) based on intent requirements.
- `S3_INTENT_CLASSIFICATION`: NLU Pipeline is classifying the user's message.
- `S4_SLOT_FILLING`: Collecting required entities (e.g., account number, amount) to fulfill an intent.
- `S5_DISAMBIGUATION`: Ambiguous input detected; actively seeking clarification from the user.
- `S6_KNOWLEDGE_RETRIEVAL`: Querying the KB for relevant information.
- `S7_RESPONSE_GENERATION`: Formulating the response payload via template or LLM.
- `S8_GUARDRAIL_CHECK`: Validating the generated response against safety rules.
- `S9_AWAITING_CUSTOMER`: Response delivered, waiting for next customer message.
- `S10_ESCALATION`: Preparing handoff context and routing to appropriate agent queue.
- `S11_HUMAN_HANDOFF`: Conversation transferred to a human agent.
- `S12_FEEDBACK_COLLECTION`: Post-resolution CSAT collection (1-5 rating + optional text).
- `S13_SESSION_CLOSED`: Conversation ended, audit log finalised.
- `S14_SESSION_TIMEOUT`: 5 minutes of inactivity detected; session frozen pending re-authentication.
- `S15_ERROR_RECOVERY`: Component failure detected; applying fallback strategy.

### State Transitions & Guards (Key Examples)
- `S3_INTENT_CLASSIFICATION` -> `S2_AUTHENTICATING`: **Guard**: Intent detected requires higher auth level than current session.
- `S4_SLOT_FILLING` -> `S6_KNOWLEDGE_RETRIEVAL`: **Guard**: All mandatory entities for the active intent are successfully extracted and validated.
- `S8_GUARDRAIL_CHECK` -> `S10_ESCALATION`: **Guard**: Critical safety violation detected (financial advice, PII leak, hallucination).
- `*` -> `S10_ESCALATION`: **Guard**: Sentiment score drops below -0.6 OR NLU confidence < threshold for 3 consecutive turns OR explicit `GEN-004` (Human Handoff Request) intent detected.
- `*` -> `S14_SESSION_TIMEOUT`: **Guard**: 5 minutes of inactivity OR critical security violation detected (e.g., repeated prompt injection attempts).

### Multi-Turn Reasoning Strategy
- **Context Window**: Maintains last 20 turns in active memory.
- **Anaphora Resolution**: Uses coreference resolution models to map pronouns ("it", "that account") to previously extracted entities.
- **Topic Switching**: Detects abrupt intent shifts, caching the previous incomplete intent to ask the user if they wish to resume it later.

## 3. Component Interface Contracts

All internal communications occur via gRPC/REST over TLS 1.3 using strictly validated JSON schemas.

### Example: NLU Pipeline Interface
**Input (from Dialogue Engine):**
```json
{
  "session_id": "uuid",
  "turn_id": "uuid",
  "raw_text": "I lost my card ending in 4567, please block it",
  "context_entities": {"account_type": "savings"}
}
```

**Output (to Dialogue Engine):**
```json
{
  "primary_intent": {"label": "CRD-001", "display": "Block/Unblock Card", "confidence": 0.95},
  "secondary_intent": {"label": "SEC-001", "display": "Report Fraud", "confidence": 0.82},
  "entities": [
    {"type": "card_last_4", "value": "4567", "confidence": 0.99, "validated": true}
  ],
  "sentiment": {"score": -0.4, "emotion": "urgency"},
  "out_of_scope": false
}
```

## 4. Failure Mode Analysis & Fallback Strategies

| Component Failure | Trigger Condition | Fallback Strategy |
| :--- | :--- | :--- |
| **NLU Confidence Low** | Primary intent confidence < 60% | Trigger `CLARIFYING` state; provide 3 likely options to user. If unresolved after 2 turns, ESCALATE. |
| **Knowledge Retrieval Fails** | Top result relevance score < threshold | Do not hallucinate. Acknowledge inability to find specific info; offer general relevant links or route to human agent. |
| **Safety Guardrail Triggered** | Hard violation (e.g., financial advice request) | Block generation. Return templated refusal response: *"I cannot provide financial advice. Please consult a certified advisor."* |
| **Third-Party API Timeout** | Core banking API latency > 1s | Graceful degradation. Return cached data if available, otherwise inform user of system delay and suggest retrying later. |

## 5. Latency Budget (< 3s End-to-End)

To ensure a seamless conversational experience, the system enforces a strict 3,000ms latency budget at the 95th percentile.

| Stage | Component | Budget (p95) |
| :--- | :--- | :--- |
| 1 | C1: Gateway Input (session lookup, language detection) | 50ms |
| 2 | C6: Input Guardrails (prompt injection scan, PII check) | 50ms (parallel with NLU) |
| 3 | C2: NLU Pipeline (intent + entities + sentiment) | 500ms |
| 4 | C3: Dialogue Policy (state transition, slot check) | 50ms |
| 5 | C4: Knowledge Retrieval (hybrid dense + BM25 + re-rank) | 200ms |
| 6 | C5: Response Generation (template or LLM) | 1,500ms |
| 7 | C6: Output Guardrails (hallucination, PII, advice check) | 100ms |
| 8 | C1: Gateway Output (format, deliver) | 50ms |
| — | C9: Audit Logging (async, fire-and-forget) | 0ms (non-blocking) |
| **Total** | **End-to-End (p95)** | **2,500ms** (500ms buffer) |

## 6. Scalability Specifications

The system is designed to be cloud-agnostic (AWS/Azure/GCP) using Kubernetes for orchestration, enabling horizontal scaling to meet varying loads.

| Scale Tier | Interaction Volume | Architecture Strategy |
| :--- | :--- | :--- |
| **1x (Current)** | 18k / day | Standard K8s cluster. Stateful conversational memory in Redis (active sessions). Postgres for async audit logs. |
| **10x Scale** | 180k / day | Horizontal Pod Autoscaling (HPA) for NLU and Generation nodes. Read-replicas for Knowledge Base (Vector DB). Multi-AZ deployment for high availability. |
| **100x Scale** | 1.8M / day | Global multi-region deployment with edge caching (CDN) for static assets/templates. Sharded Redis clusters for session state. Dedicated GPU inference clusters for LLMs. Kafka for buffering massive audit log streams to data lakes. |
