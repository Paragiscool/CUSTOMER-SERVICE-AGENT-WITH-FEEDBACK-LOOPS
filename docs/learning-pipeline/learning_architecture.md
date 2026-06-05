# Continuous Learning Architecture & A/B Testing Pipeline

The NexBank Agentic AI uses an asynchronous, batch-updated continuous learning pipeline driven by explicit signals. This ensures the model adapts to new user behaviors without the extreme compliance risks associated with "live" or online learning in a banking production environment.

## 1. The Closed-Loop Improvement Cycle

The pipeline operates in 5 distinct stages to guarantee safety and data integrity:

### Stage 1: Data Collection & Privacy Gate
- **Function**: Collect user interactions and system metrics.
- **Privacy Gate**: Before entering the Training Data Lake, all PII (Aadhaar, PAN, Card Numbers) is scrubbed. Transaction amounts and specific dates are mapped to synthetic equivalents to preserve semantic intent without exposing real financial data.

### Stage 2: Signal Aggregation
- **Function**: Categorize feedback into actionable metrics.
- **Explicit Signals**: Post-chat Customer Satisfaction (CSAT) surveys, thumbs up/down on specific responses.
- **Implicit Signals**: Conversation length, user drop-off rates, intent fallback frequency, and escalation-to-human rates.

### Stage 3: Supervisor Review (Human-in-the-Loop)
- **Function**: Ensure high-quality ground truth.
- **Process**: Low-confidence interactions and failed test cases are automatically flagged for human review. Supervisors manually correct the intent mapping, entity extraction, or generated response in the annotation tool.

### Stage 4: Model Update (LoRA/PEFT)
- **Function**: Safely adapt the model.
- **Process**: Approved corrections are batched. The system uses Parameter-Efficient Fine-Tuning (PEFT) specifically utilizing Low-Rank Adaptation (LoRA). This updates the model's behavior towards the new ground truth without causing catastrophic forgetting of its original safety alignment.

### Stage 5: Safety Invariant Testing (The Air Gap)
- **Function**: *Crucial Go/No-Go Gate.*
- **Process**: The newly tuned LoRA weights are merged into a staging model. This model **MUST** pass the fixed 200-case safety and adversarial test suite (including the 60+ generated in Phase 2). 
- **Rule**: If it fails a single safety test (e.g., outputs prohibited advice), the update is categorically rejected and flagged for MLOps analysis.

---

## 2. A/B Testing & Deployment Governance

To ensure new model variants improve the user experience without breaking existing flows, deployment follows a strict A/B testing framework.

### Deployment Strategy

1. **Shadow Mode**: The new model variant runs parallel to the production model. It processes live queries, but its responses are recorded silently and NOT shown to the user. Outputs are compared against the control for latency, formatting, and semantic similarity.
2. **Canary Release**: If Shadow Mode passes (no latency spikes or formatting errors), the variant is exposed to a small percentage (e.g., 5%) of live traffic.
3. **A/B Evaluation**: Traffic is split using the Gateway Router. We measure key business metrics (CSAT, Resolution Rate, Escalation Rate) between the Control (Current Model) and Variant (New Model) over a 48-hour window.
4. **Full Rollout & Auto-Rollback**: 
   - *Win*: If the Variant wins with statistical significance, it is promoted to the new Control.
   - *Loss/Breach*: If the Variant degrades performance (e.g., CSAT drops by >5%) or violates a real-time guardrail, an **automated rollback** instantly reverts all traffic to the Control within 30 seconds. This auto-rollback mechanism is non-negotiable.
