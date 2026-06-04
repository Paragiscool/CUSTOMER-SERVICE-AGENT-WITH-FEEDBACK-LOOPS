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

### Core States
- `INIT`: Session started, awaiting initial user input or authentication.
- `AUTHENTICATING`: Verifying user identity (OTP, Biometric) based on intent requirements.
- `CLARIFYING`: Ambiguous input detected; actively seeking disambiguation from the user.
- `SLOT_FILLING`: Collecting required entities (e.g., account number, amount) to fulfill an intent.
- `RETRIEVING_KNOWLEDGE`: Querying the KB for relevant information.
- `GENERATING_RESPONSE`: Formulating the response payload.
- `ESCALATING`: Handing off to a human agent.
- `RESOLVED`: Intent fulfilled successfully.
- `TERMINATED`: Session timed out, completed, or hard-stopped due to safety violation.

### State Transitions & Guards
- `INIT` -> `AUTHENTICATING`: **Guard**: Intent detected requires higher auth level than current session.
- `SLOT_FILLING` -> `GENERATING_RESPONSE`: **Guard**: All mandatory entities for the active intent are successfully extracted and validated.
- `*` -> `ESCALATING`: **Guard**: Sentiment score drops below -0.6 OR NLU confidence < threshold for 2 consecutive turns OR explicit human request OR priority intent detected.
- `*` -> `TERMINATED`: **Guard**: 5 minutes of inactivity OR critical security violation detected (e.g., repeated prompt injection attempts).

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
  "primary_intent": {"label": "security", "confidence": 0.98},
  "secondary_intent": {"label": "card_blocking", "confidence": 0.95},
  "entities": [
    {"type": "card_number_partial", "value": "4567", "confidence": 0.99, "validated": true}
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

- **Gateway & Network Overhead**: 100ms
- **Dialogue Engine Processing**: 50ms
- **NLU Pipeline (Intent + Entities + Sentiment)**: 450ms
- **Guardrail Pre-Check**: 100ms
- **Knowledge Retrieval / API Fetch**: 200ms
- **Response Generation (LLM/Hybrid)**: 1,800ms
- **Guardrail Post-Check**: 150ms
- **Audit Logging (Async)**: 0ms (Non-blocking)
- **Total Budget**: 2,850ms (leaves 150ms buffer)

## 6. Scalability Specifications

The system is designed to be cloud-agnostic (AWS/Azure/GCP) using Kubernetes for orchestration, enabling horizontal scaling to meet varying loads.

| Scale Tier | Interaction Volume | Architecture Strategy |
| :--- | :--- | :--- |
| **1x (Current)** | 18k / day | Standard K8s cluster. Stateful conversational memory in Redis (active sessions). Postgres for async audit logs. |
| **10x Scale** | 180k / day | Horizontal Pod Autoscaling (HPA) for NLU and Generation nodes. Read-replicas for Knowledge Base (Vector DB). Multi-AZ deployment for high availability. |
| **100x Scale** | 1.8M / day | Global multi-region deployment with edge caching (CDN) for static assets/templates. Sharded Redis clusters for session state. Dedicated GPU inference clusters for LLMs. Kafka for buffering massive audit log streams to data lakes. |
