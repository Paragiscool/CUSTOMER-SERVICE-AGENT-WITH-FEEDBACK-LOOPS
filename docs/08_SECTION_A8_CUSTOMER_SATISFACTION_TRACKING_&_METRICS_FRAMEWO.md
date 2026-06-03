SECTION A8: CUSTOMER SATISFACTION TRACKING & METRICS FRAMEWORK 
A8.1 Leading Indicators 
Metric 
Definition 
Target 
Frequency 
First Response Time 
Time from customer 
message to first agent 
response 
<5 seconds 
Real-time 
Intent Recognition 
Accuracy 
Percentage of intents 
correctly classified on 
first attempt 
>92% 
Daily 
Entity Extraction 
Accuracy 
Percentage of entities 
correctly extracted and 
validated 
>95% 
Daily 
Sentiment Trajectory 
Slope 
Average rate of sentiment 
change during 
conversations 
Positive or 
stable 
Daily 
Knowledge Retrieval 
Relevance 
Average relevance score 
of retrieved knowledge 
items 
>0.85 
Daily 
Guardrail False Positive 
Rate 
Percentage of safe 
responses blocked by 
guardrails 
<3% 
Daily 
Agent Confidence 
Distribution 
Distribution of confidence 
scores across all 
interactions 
>80% above 0.7 
Weekly 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 19 


## Page 20

 
 
 
 
A8.2 Lagging Indicators 
Metric 
Definition 
Target 
Frequency 
CSAT Score 
Average post-conversation 
rating (1-5 scale) 
≥4.5 
Weekly 
Net Promoter Score 
(NPS) 
Likelihood to recommend 
NexBank AI support 
≥50 
Monthly 
First Contact Resolution 
(FCR) 
Issues resolved without 
escalation or return 
≥70% 
Weekly 
Containment Rate 
Conversations fully 
handled by AI 
≥70% 
Weekly 
Customer Effort Score 
(CES) 
Ease of issue resolution 
(1-7 scale) 
≥5.5 
Monthly 
Incorrect Advice Rate 
Conversations containing 
wrong financial info 
0% 
Daily 
Customer Retention 
Impact 
Correlation between AI 
support experience and 
retention 
Positive 
Quarterly 
 
A8.3 Operational Metrics 
Metric 
Definition 
Target 
Frequency 
Average Handle Time 
Conversation duration 
from start to resolution 
<8 minutes 
Daily 
Escalation Rate 
Conversations requiring 
human handoff 
<30% 
Daily 
System Uptime 
AI agent availability and 
responsiveness 
≥99.9% 
Real-time 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 20 


## Page 21

 
 
 
 
Concurrent Capacity 
Simultaneous 
conversations without 
degradation 
≥10,000 
Weekly load test 
Knowledge Base 
Coverage 
Customer queries 
addressable by current KB 
≥90% 
Monthly 
Model Drift Score 
Performance drift from 
baseline 
<0.05 
Weekly 
 
A8.4 Improvement Cycle Design 
Candidates must specify a closed-loop improvement cycle connecting metrics to actions: 
●​ DETECT: Automated monitoring dashboards surfacing metric anomalies and 
performance degradation patterns within 15 minutes of occurrence. 
●​ DIAGNOSE: Root cause analysis framework connecting metric degradation to specific 
system components (NLU accuracy, knowledge gaps, generation quality, escalation 
logic). 
●​ DESIGN: Improvement hypothesis generation and prioritisation framework based on 
impact (metric improvement potential), effort (engineering resources), and risk (potential 
for unintended consequences). 
●​ DEPLOY: Staged rollout with A/B testing for all improvements affecting 
customer-facing behaviour. 
●​ VALIDATE: Post-deployment monitoring confirming improvement hypothesis and 
detecting unintended side effects. 
●​ DOCUMENT: Knowledge capture documenting what was learned from each 
improvement cycle for institutional memory. 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 21 


## Page 22

 
 
 
 
A8.5 Dashboard Specification 
Real-Time Operations Dashboard: 
●​ Live conversation volume, active escalations, system health indicators 
●​ Rolling 1-hour CSAT, containment rate, and escalation rate 
●​ Guardrail trigger heat map showing most frequently activated safety boundaries 
●​ Alert panel for anomalous patterns (fraud report spikes, unusual escalation volume, 
system errors) 
 
Daily Performance Dashboard: 
●​ 24-hour trend lines for all leading and lagging indicators 
●​ Conversation funnel analysis: started > engaged > resolved > satisfied 
●​ Top 10 unresolved intents and top 10 low-confidence interactions for review 
●​ Supervisor correction summary with categorisation of correction types 
 
Weekly Strategic Dashboard: 
●​ Week-over-week improvement trends for all KPIs 
●​ A/B test results and pending experiment pipeline 
●​ Knowledge base gap analysis with recommended updates 
●​ Competitive benchmarking against industry metrics 
●​ Learning pipeline metrics: training data volume, model version history, safety test pass 
rates 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 22 


## Page 23

 
 
 
 
