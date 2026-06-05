# Safety Invariant Testing Protocol

Continuous learning poses a unique risk: fine-tuning a model to be more helpful and conversational can inadvertently cause it to "unlearn" its strict safety and compliance alignments (a phenomenon known as catastrophic forgetting). 

To prevent this, the NexBank system employs an **Immutable Safety Layer** and a strict **Safety Invariant Protocol** during the deployment phase.

## 1. The Immutable Safety Layer

The core safety instructions (e.g., "Never provide financial advice," "Never leak PII") are hardcoded into the base system prompt and the Pre/Post-Flight Guardrails. 

- **LoRA Isolation**: The Continuous Learning pipeline only updates the Low-Rank Adaptation (LoRA) weights of the primary LLM. These weights are designed to influence tone, intent routing, and formatting.
- **Rule of Precedence**: The system prompt and guardrail middleware *always* supersede the LoRA weights. Even if a newly tuned model attempts to output prohibited text, the Post-Flight scanner will intercept it.

## 2. The Air Gap: 200-Case Invariant Test Suite

Before any newly fine-tuned model variant can enter "Shadow Mode" or "Canary Release", it must pass through an automated "Air Gap" testing environment.

### The Test Suite
The system maintains a fixed, non-negotiable suite of 200 adversarial and safety-critical test cases (including the 60 defined in Phase 2). This suite covers:
1. Direct Prompt Injection (Jailbreaks)
2. Social Engineering / Empathy Exploitation
3. Prohibited Financial Advice Solicitation
4. Multi-Intent Complex Queries
5. Edge-case mathematical reasoning

### The Evaluation Criteria
The staging model processes all 200 cases. 
- **Expected Outcome**: The model must block, deflect, or correctly answer 100% of the safety-critical cases.
- **Tolerance**: **0%**. There is zero tolerance for safety degradation. 

### Automated Rejection
If the new LoRA weights cause the model to fail *even a single* safety invariant test (e.g., it offers advice on one prompt where the previous control model correctly deflected), the deployment pipeline is automatically aborted. 

The build is marked as `FAILED_SAFETY_INVARIANT`, an alert is sent to the MLOps team, and the model must be retrained with a higher penalty on safety violations. This guarantees that as the model gets "smarter" or more empathetic over time, it never becomes less safe.
