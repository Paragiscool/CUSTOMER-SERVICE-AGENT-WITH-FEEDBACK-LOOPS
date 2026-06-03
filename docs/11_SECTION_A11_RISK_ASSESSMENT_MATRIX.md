SECTION A11: RISK ASSESSMENT MATRIX 
A11.1 Technical Risk Matrix 
Risk 
Prob. 
Impact 
Severity 
Mitigation Required 
LLM hallucination 
producing incorrect 
financial info 
Medium 
Critical 
P0 
Response grounding, KB 
cross-check, financial figure 
validation 
Model drift 
degrading 
performance 
High 
High 
P1 
Continuous monitoring, drift 
detection, retraining triggers 
Context window 
overflow in complex 
conversations 
Medium 
Medium 
P2 
Summarisation, sliding window, 
priority retention 
KB staleness from 
delayed regulatory 
updates 
Medium 
Critical 
P0 
Regulatory fast-track, staleness 
alerts, version-tagged KB with 
expiry 
Cascading failures 
across components 
Low 
Critical 
P1 
Circuit breaker, bulkhead 
isolation, rule-based fallback 
Latency spikes 
during peak hours 
Medium 
Medium 
P2 
Auto-scaling, request queuing, 
response caching, CDN for 
static KB 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 30 


## Page 31

 
 
 
 
A11.2 Business Risk Matrix 
Risk 
Prob. 
Impact 
Severity 
Mitigation Required 
Customer churn from 
poor AI experience 
Medium 
High 
P1 
Real-time CSAT, auto-escalation 
on negative sentiment, A/B 
testing 
Regulatory penalty 
for non-compliant AI 
Low 
Critical 
P0 
Compliance layer, mandatory 
testing, quarterly regulatory 
audit 
Reputational damage 
from viral negative 
interaction 
Low 
Critical 
P1 
Quality monitoring, crisis 
response, detect potentially viral 
conversations 
Legal liability from 
AI-caused customer 
loss 
Low 
Critical 
P0 
Comprehensive guardrails, legal 
review, insurance, audit trail 
 
A11.3 Ethical Risk Assessment 
Fairness & Bias: 
●​ Agent must not exhibit differential treatment based on demographics, balance, or history 
●​ Escalation thresholds must be consistent across all customer segments 
●​ Product recommendations must not use discriminatory criteria 
●​ Candidates must specify bias detection and mitigation including statistical fairness testing 
 
Transparency: 
●​ Customers must be informed they are interacting with AI at conversation start 
●​ Agent must never deny being AI or claim to be human 
●​ Agent must acknowledge uncertainty rather than presenting guesses as facts 
●​ Customers must have the right to request human assistance at any point 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 31 


## Page 32

 
 
 
 
Accountability: 
●​ Every AI decision must be traceable to specific inputs, model versions, and KB entries 
●​ Accountability chain documented: who designed, who approved the model, who 
reviewed guardrails 
●​ Incident response procedures identify responsible parties for different failure modes 
 
Privacy: 
●​ Collect only minimum information necessary to resolve customer query 
●​ Training data must be anonymised with customer consent 
●​ Never infer or store sensitive attributes (religion, politics, health) from conversations 
●​ Cross-customer 
data 
leakage 
must 
be 
architecturally 
impossible, 
not 
just 
policy-prohibited 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 32 


## Page 33

 
 
 
 
