# Risk Assessment Matrix

This document outlines the technical, business, and ethical risks associated with the NexBank Agentic AI deployment, mapping each risk to its severity and required architectural mitigations.

## 1. Technical Risk Matrix

| Risk | Probability | Impact | Severity | Mitigation Required |
|---|---|---|---|---|
| **LLM hallucination producing incorrect financial info** | Medium | Critical | P0 | Response grounding (RAG), KB cross-check, financial figure validation via deterministic templates. |
| **Model drift degrading performance** | High | High | P1 | Continuous monitoring, drift detection (Model Drift Score), retraining triggers in the Continuous Learning Pipeline. |
| **Context window overflow in complex conversations** | Medium | Medium | P2 | Summarization, sliding window memory, priority retention of extracted entities. |
| **KB staleness from delayed regulatory updates** | Medium | Critical | P0 | Regulatory fast-track update pipeline, staleness alerts, version-tagged KB chunks with strict expiry dates. |
| **Cascading failures across components** | Low | Critical | P1 | Circuit breaker pattern, bulkhead isolation (especially for Gateway C1), rule-based fallback mode. |
| **Latency spikes during peak hours** | Medium | Medium | P2 | Auto-scaling (10x to 100x architectures), request queuing, speculative response caching, CDN for static KB data. |

## 2. Business Risk Matrix

| Risk | Probability | Impact | Severity | Mitigation Required |
|---|---|---|---|---|
| **Regulatory penalty for non-compliant AI** | Low | Critical | P0 | Compliance layer (Layer 2 Prompt), mandatory testing, quarterly regulatory audit, RBI reporting. |
| **Legal liability from AI-caused customer loss** | Low | Critical | P0 | Comprehensive guardrails (Financial Advice Prohibition), legal review of prompts, immutable safety invariant protocol, audit trail. |
| **Customer churn from poor AI experience** | Medium | High | P1 | Real-time CSAT tracking, auto-escalation on negative sentiment, A/B testing of all dialogue changes. |
| **Reputational damage from viral negative interaction** | Low | Critical | P1 | Quality monitoring, crisis response templates (ESC-011), anomaly detection to flag potentially viral conversations. |

## 3. Ethical Risk Assessment

### Fairness & Bias
- **Risk**: The AI treats customers differently based on demographics or account balance.
- **Mitigation**: 
  - The agent must not exhibit differential treatment based on demographics.
  - Escalation thresholds must be consistent across all customer segments (P0/P1/P2 routing applies universally).
  - Product recommendations must not use discriminatory criteria.
  - Required statistical fairness testing on all new LoRA weight updates.

### Transparency
- **Risk**: Customers believe they are speaking to a human, or the AI fabricates a guess as a fact.
- **Mitigation**:
  - Customers must be informed they are interacting with AI at the conversation start (Template PT-01).
  - The agent must never deny being AI or claim to be human.
  - The agent must acknowledge uncertainty (Graceful Degradation pattern) rather than presenting guesses as facts.
  - Customers must have the right to request human assistance at any point (ESC-006 triggers after 3 requests).

### Accountability
- **Risk**: The bank cannot determine why the AI made a specific decision.
- **Mitigation**:
  - Every AI decision must be traceable to specific inputs, model versions, and KB entries via the Turn-Level Log Schema.
  - Accountability chain documented in Git/version control: who designed the prompt, who approved the model, who reviewed the guardrails.
  - Incident response procedures identify responsible parties for different failure modes.

### Privacy
- **Risk**: The AI leaks one customer's data to another, or ingests sensitive data into its training weights.
- **Mitigation**:
  - Collect only the minimum information necessary to resolve the query.
  - Training data must be anonymized (PII Ingestion Gate) with customer consent.
  - Never infer or store sensitive attributes (religion, politics, health) from conversations.
  - Cross-customer data leakage is architecturally impossible (each session runs in an isolated ephemeral state container).
