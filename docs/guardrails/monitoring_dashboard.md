# Guardrail Monitoring & False Positive Dashboard

To ensure the safety mechanisms do not degrade the customer experience, the system continuously monitors guardrail interventions. This dashboard provides real-time visibility into system security and false positive rates.

## Dashboard Components

### 1. Real-Time Security Tracking
Tracks active threats and interventions in the production environment.
- **Injection Attempt Rate**: Spikes indicate a coordinated adversarial attack or a newly published jailbreak being tested by users.
- **Pre-Flight Rejection %**: Percentage of queries blocked before reaching the LLM (due to intent or PII).
- **Post-Flight Modification %**: Percentage of generated responses that had to be altered or dropped by the output scanner.

### 2. False Positive Analysis (Customer Friction)
Overly aggressive guardrails ruin user experience. A "False Positive" occurs when a legitimate customer query is incorrectly flagged as a security threat or a request for financial advice.
- **Deflection Escalation Rate**: How often a user escalates to a human agent immediately after the AI refuses to answer due to a guardrail. High rates here indicate the guardrail is blocking legitimate information.
- **Frustration Sentiment Spikes**: If sentiment drops sharply following a guardrail disclaimer, the system flags the interaction for review.

### 3. Review & Tuning Workflow
- **Daily Sampling**: MLOps samples 100 sessions where the `request_advice` classifier triggered.
- **Annotation**: Human annotators verify if the user was actually asking for advice or just requesting complex factual information.
- **Threshold Tuning**: If the False Positive rate exceeds 2%, the confidence threshold for the intent classifiers and the sensitivity of the Post-Flight regex filters are recalibrated.

## Visual Layout Specification (Grafana/Kibana)
- **Top Row (KPIs)**: Total Sessions, Guardrail Interventions (Count), False Positive Rate (%), Average Response Latency Impact (ms).
- **Middle Row (Trends)**: Time-series graph showing "Permissible Info" vs "Blocked Advice" over the last 7 days.
- **Bottom Row (Alerts)**: Streaming log of recent high-severity interventions (e.g., suspected RAG poisoning, repetitive brute force authentication failures).
