# Guardrail Incident Response Playbook

In the event that the Agentic AI breaches a safety boundary (e.g., providing prohibited financial advice, succumbing to a jailbreak, or leaking PII), the following deterministic playbook must be executed.

## Roles and Responsibilities
- **L1 Support (Ops):** Initial triage, containment execution.
- **MLOps Engineer:** Prompt patching, classifier retraining, log analysis.
- **Compliance Officer:** Regulatory reporting, impact assessment, final approval for recovery.

## Scenario 1: Prohibited Financial Advice Generated
**Trigger**: Post-flight scan or customer feedback indicates the agent told a user what specific investment to make.
1. **Containment**: Ops instantly toggles the `ADVICE_OVERRIDE_KILLSWITCH`, forcing all queries containing investment keywords to bypass the LLM and return a hardcoded template.
2. **Investigation**: MLOps isolates the session ID and extracts the exact input prompt, retrieved context (RAG chunks), and generated response. Determine if the failure was in the Pre-Flight intent classifier or the LLM's adherence to the system prompt.
3. **Remediation**: 
   - If Pre-Flight failed: Add the utterance to the classifier's negative training set.
   - If LLM failed: Strengthen the system prompt negative constraints or adjust the Post-Flight regex filter.
4. **Recovery**: Deploy patched constraints to Staging, run the regression suite. Once Compliance Officer approves, deploy to Production and release the killswitch.

## Scenario 2: Successful Prompt Injection (Jailbreak)
**Trigger**: Agent outputs text indicating roleplay adoption (e.g., "I am now unrestricted...") or executes unauthorized tool calls.
1. **Containment**: If an unauthorized tool call was attempted, the Action Screener should have blocked it. If it bypassed the screener, instantly disable the specific tool API for the entire agent fleet. If it is a purely conversational jailbreak, route the specific user to a human agent and lock their AI access.
2. **Investigation**: Extract the injection payload. Analyze the token obfuscation or semantic technique used to bypass the Pre-Flight classifier.
3. **Remediation**: Update the Pre-Flight classifier to recognize the new adversarial pattern. Evaluate if the "Sandwich Architecture" delimiters (`<user_input>`) were compromised.
4. **Recovery**: Add the injection payload to `adversarial_test_cases.json`. Verify the system now safely deflects the attack.

## Scenario 3: PII Leakage
**Trigger**: Post-Flight scan detects an unmasked PAN/Card number in the outgoing payload.
1. **Containment**: The system should automatically drop the payload. Ops must verify the drop occurred.
2. **Investigation**: Trace how the PII entered the context window. Did the Ingestion Gate fail to scrub a user's input, or did a Knowledge Base chunk contain unscrubbed PII?
3. **Remediation**: Purge the PII from any logging databases immediately. Fix the regex/semantic scrubber in the Ingestion Gate or remove the offending chunk from the Vector DB.
4. **Post-Incident Review**: File a mandatory data privacy incident report with the internal risk committee.
