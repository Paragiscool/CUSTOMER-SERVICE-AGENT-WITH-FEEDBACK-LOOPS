SECTION A10: AUDIT & COMPLIANCE LOGGING SPECIFICATION 
A10.1 Interaction Log Schema 
Each conversation must produce a structured log record: 
Field 
Type 
Description 
Retention 
conversation_id 
UUID 
Unique identifier for the 
conversation session 
7 years 
customer_id 
UUID 
(hashed) 
Pseudonymised customer 
identifier 
7 years 
channel 
Enum 
Communication channel (chat, 
voice, WhatsApp, app) 
7 years 
authentication_level 
Enum 
Highest authentication level 
achieved 
7 years 
primary_intent 
String 
Final classified intent for the 
conversation 
7 years 
resolution_status 
Enum 
resolved_ai, resolved_human, 
unresolved, escalated 
7 years 
guardrails_activated 
Array[String] 
List of guardrails that fired during 
conversation 
7 years 
csat_score 
Float (1-5) 
Customer satisfaction rating if 
provided 
3 years 
model_version 
String 
AI model version that handled 
conversation 
7 years 
turn_count 
Integer 
Total number of conversation 
turns 
7 years 
pii_detected 
Boolean 
Whether PII was detected in 
customer messages 
7 years 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 27 


## Page 28

 
 
 
 
A10.2 Turn-Level Log Schema 
In addition to conversation-level logging, each turn must be individually logged with: turn_id, 
speaker (customer/agent/system), encrypted raw_message with PII redacted, intent_classification 
(label, confidence, alternatives), entities_extracted (type, value, confidence, validation), 
sentiment_score (-1 to 1), knowledge_items_retrieved, retrieval_confidence, agent_response, 
response_generation_method 
(template/llm/hybrid), 
guardrails_checked 
and 
guardrails_triggered, response_latency_ms, and action_taken (respond, clarify, confirm, escalate, 
authenticate). 
 
A10.3 Compliance Requirements 
Data Encryption: 
●​ All log data encrypted at rest using AES-256 encryption 
●​ All log data encrypted in transit using TLS 1.3 
●​ PII fields additionally encrypted with separate key (double encryption) 
●​ Encryption keys rotated quarterly through hardware security module (HSM) 
 
Access Control: 
●​ Log access follows principle of least privilege 
●​ Customer-facing staff access only anonymised, aggregated metrics 
●​ Supervisors access full logs only for conversations in their review queue 
●​ All log access itself is logged (meta-audit trail) 
 
Data Retention & Deletion: 
●​ Retention periods per schema must be enforced automatically 
●​ Customer right-to-deletion requests processed within 30 days 
●​ Deletion includes all copies, backups, and derived data including model training data 
●​ Deletion certificates generated and stored for regulatory proof 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 28 


## Page 29

 
 
 
 
Regulatory Reporting: 
●​ Monthly AI interaction summary for RBI (volume, escalation rates, complaint categories) 
●​ Quarterly customer complaint analysis per Banking Ombudsman requirements 
●​ Annual AI system audit report covering accuracy, fairness, and safety metrics 
●​ Ad-hoc regulatory query responses within 48 hours of receipt 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 29 


## Page 30

 
 
 
 
