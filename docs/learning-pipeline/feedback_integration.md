# Feedback Integration & Signal Collection

To power the Continuous Learning Pipeline, the Agentic AI relies on a structured taxonomy of explicit and implicit feedback signals. These signals are collected by the Dialogue Management Engine and routed to the Signal Aggregation database.

## 1. Explicit Feedback (Customer Satisfaction - CSAT)

Direct feedback from the user provides the highest fidelity signal for model improvement.

- **Micro-Feedback (Turn-Level)**: After safety-critical or complex answers (e.g., explaining a loan calculation), the UI presents a subtle 👍/👎 mechanism. A 👎 prompts an optional "What went wrong?" multi-choice (Inaccurate, Confusing, Irrelevant).
- **Macro-Feedback (Session-Level)**: Upon session conclusion or handoff, a standard 5-point CSAT survey is presented: *"How satisfied are you with the AI Assistant today?"*
  - Scores 1-2 (Dissatisfied) automatically flag the entire session transcript for Human-in-the-Loop (HITL) Supervisor Review.

## 2. Implicit Feedback (Behavioral Metrics)

Because survey response rates are typically low (10-15%), implicit behavioral signals are heavily weighted in the model update strategy.

- **Resolution Outcome Tracking**:
  - *Resolved*: User explicitly confirms resolution or closes the app gracefully after an answer.
  - *Abandoned*: User drops off mid-conversation without completing a required slot-filling task.
- **Repeat Contact Monitoring**: If a user contacts the agent again within 24 hours regarding the same intent domain (e.g., `Card Management`), the previous session is marked as *Unresolved*.
- **Escalation Rate**: Tracking the percentage of conversations that trigger the `Escalation Router` (C7). A sudden spike in escalations for a specific intent (e.g., `Reset PIN`) indicates the model's instructions for that intent are failing.

## 3. The Supervisor Correction Taxonomy

When a session is flagged for HITL review, the Supervisor uses an internal annotation tool to classify the failure. This taxonomy directly informs the LoRA fine-tuning objective.

| Failure Category | Description | Fine-Tuning Action |
|---|---|---|
| **Intent Misclassification** | Agent routed the query to the wrong domain (e.g., thought a "balance transfer" meant a credit card feature instead of a wire transfer). | Add the utterance to the NLU intent training set. |
| **Entity Extraction Failure** | Agent failed to capture the date or amount provided by the user. | Add edge-case regex or slot-filling examples to the NLU model. |
| **Tone / Empathy Failure** | Response was factually correct but overly robotic or lacked empathy during a stressful query. | Fine-tune the response generation LLM with rewritten, empathetic examples. |
| **Hallucination / KB Miss** | Agent provided an incorrect rate or hallucinated a policy. | Do NOT fine-tune the LLM. Update the Vector DB / RAG chunks. |

By separating *Knowledge Failures* from *Behavioral/Tone Failures*, the MLOps team ensures the LLM is only fine-tuned to improve reasoning and tone, while factual accuracy is strictly managed via the Retrieval pipeline.
