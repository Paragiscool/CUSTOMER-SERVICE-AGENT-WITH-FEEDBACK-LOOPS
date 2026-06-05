# Customer Satisfaction Tracking & Metrics Framework

The NexBank Agentic AI uses a structured metrics framework organized into Leading, Lagging, and Operational indicators to ensure continuous performance visibility and drive data-informed improvements.

## 1. Leading Indicators

Leading indicators provide early warning signals before customer satisfaction degrades.

| Metric | Definition | Target | Frequency |
|---|---|---|---|
| **First Response Time** | Time from customer message to first agent response. | <5 seconds | Real-time |
| **Intent Recognition Accuracy** | Percentage of intents correctly classified on first attempt. | >92% | Daily |
| **Entity Extraction Accuracy** | Percentage of entities correctly extracted and validated. | >95% | Daily |
| **Sentiment Trajectory Slope** | Average rate of sentiment change during conversations. | Positive or stable | Daily |
| **Knowledge Retrieval Relevance** | Average relevance score of retrieved knowledge items (re-ranker output). | >0.85 | Daily |
| **Guardrail False Positive Rate** | Percentage of safe responses incorrectly blocked by guardrails. | <3% | Daily |
| **Agent Confidence Distribution** | Distribution of NLU confidence scores across all interactions. | >80% above 0.7 | Weekly |

## 2. Lagging Indicators

Lagging indicators measure actual customer outcomes and business impact.

| Metric | Definition | Target | Frequency |
|---|---|---|---|
| **CSAT Score** | Average post-conversation rating (1-5 scale). | ≥4.5 | Weekly |
| **Net Promoter Score (NPS)** | Likelihood to recommend NexBank AI support. | ≥50 | Monthly |
| **First Contact Resolution (FCR)** | Issues resolved without escalation or return visit within 24 hours. | ≥70% | Weekly |
| **Containment Rate** | Conversations fully handled by AI without human handoff. | ≥70% | Weekly |
| **Customer Effort Score (CES)** | Ease of issue resolution (1-7 scale). | ≥5.5 | Monthly |
| **Incorrect Advice Rate** | Conversations containing wrong financial information. | 0% | Daily |
| **Customer Retention Impact** | Correlation between AI support experience and customer retention. | Positive | Quarterly |

## 3. Operational Metrics

Operational metrics track system health and capacity.

| Metric | Definition | Target | Frequency |
|---|---|---|---|
| **Average Handle Time** | Conversation duration from start to resolution. | <8 minutes | Daily |
| **Escalation Rate** | Conversations requiring human handoff. | <30% | Daily |
| **System Uptime** | AI agent availability and responsiveness. | ≥99.9% | Real-time |
| **Concurrent Capacity** | Simultaneous conversations without degradation. | ≥10,000 | Weekly load test |
| **Knowledge Base Coverage** | Customer queries addressable by current KB. | ≥90% | Monthly |
| **Model Drift Score** | Performance drift from baseline. | <0.05 | Weekly |

## 4. Closed-Loop Improvement Cycle

The metrics framework is connected to action through a 6-stage improvement cycle:

### Stage 1: DETECT
Automated monitoring dashboards surface metric anomalies and performance degradation patterns within **15 minutes** of occurrence. Alert thresholds are configured for each metric (e.g., CSAT drops below 4.0 triggers an immediate alert).

### Stage 2: DIAGNOSE
Root cause analysis framework connects metric degradation to specific system components:
- Low Intent Accuracy → NLU Pipeline (C2) issue
- Low KB Relevance → Knowledge Base (C4) gap or stale data
- High Escalation Rate → Escalation Router (C7) thresholds too aggressive
- Low CSAT despite high accuracy → Response Engine (C5) tone/empathy issue

### Stage 3: DESIGN
Improvement hypothesis generation and prioritization using an **Impact × Effort × Risk** scoring matrix:
- **Impact**: Expected metric improvement (quantified).
- **Effort**: Engineering resources required (story points).
- **Risk**: Potential for unintended consequences (e.g., improving empathy might loosen safety boundaries).

### Stage 4: DEPLOY
Staged rollout via the A/B testing framework defined in `learning_architecture.md`. All improvements affecting customer-facing behavior must pass through Shadow Mode → Canary → A/B Evaluation.

### Stage 5: VALIDATE
Post-deployment monitoring for 48 hours confirming the improvement hypothesis. The system checks for unintended side effects (e.g., a change that improved CSAT but increased hallucination rate).

### Stage 6: DOCUMENT
Knowledge capture documenting what was learned from each improvement cycle. Stored in the `CHANGELOG.md` and an internal "Lessons Learned" registry for institutional memory.
