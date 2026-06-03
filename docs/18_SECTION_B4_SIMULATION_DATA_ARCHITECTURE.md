SECTION B4: SIMULATION DATA ARCHITECTURE 
The NexBank AI Architect simulation operates on a multi-layered data model that mirrors 
real-world banking AI system design. Understanding this data architecture is essential for 
designing realistic specifications. 
B4.1 Customer Interaction Data Model 
Data Layer 
Content 
Usage in Simulation 
Customer Profile 
Demographics, 
product holdings, 
segment, PEP flag 
Drives authentication requirements, product 
eligibility checks, escalation triggers for 
high-value customers 
Interaction History 
Past conversations, 
complaints, CSAT 
scores, channel 
preferences 
Enables context carry-over pattern, repeat 
contact detection, personalised escalation 
thresholds 
Transaction Ledger 
Recent transactions, 
recurring payments, 
dispute history 
Required for TXN intents, fraud pattern 
detection, dispute investigation scenarios 
Product Catalogue 
NexBank products, 
rates, eligibility 
criteria, regulatory 
tags 
Knowledge base source for PRD intents, 
guardrail-bounded product information 
delivery 
Regulatory Registry 
RBI circulars, SEBI 
guidelines, PCI DSS 
rules, compliance 
metadata 
Regulatory knowledge updates, compliance 
guardrail validation, audit trail requirements 
Agent Performance 
Model versions, 
confidence 
distributions, 
guardrail trigger 
rates, drift scores 
Continuous learning pipeline inputs, A/B test 
metrics, safety regression monitoring 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 45 


## Page 46

 
 
 
 
B4.2 Simulation Scoring Engine 
The scoring engine evaluates candidate deliverables against the following weighted criteria at 
each Campaign level: 
●​ Completeness Score (30%): Are all mandatory elements present? Does the specification 
address every requirement listed in the level mission? 
●​ Depth Score (25%): Does the specification go beyond surface-level descriptions? Are 
there implementation-ready details, edge cases considered, failure modes analysed? 
●​ Correctness Score (20%): Are technical claims accurate? Are regulatory references 
correct? Are latency budgets realistic? 
●​ Innovation Score (15%): Does the candidate propose novel approaches, identify 
unspecified edge cases, or demonstrate creative problem-solving? 
●​ Coherence Score (10%): Does the specification integrate cleanly with other system 
components? Are there contradictions or inconsistencies? 
 
B4.3 Leaderboard & Rankings 
The simulation maintains three leaderboards tracking different aspects of candidate performance: 
Campaign Leaderboard (Primary): 
●​ Ranked by total Campaign points (maximum 1000) 
●​ Tiebreaker 1: Safety Points (higher safety score wins) 
●​ Tiebreaker 2: Time to completion (faster wins) 
●​ Updated in real-time as candidates complete levels 
 
Arena Leaderboard (Competitive): 
●​ ELO-style rating starting at 1200, adjusted after each arena challenge 
●​ Win/loss record tracked across all arena challenge types 
●​ Weekly ranking reset with season-long cumulative tracking 
●​ Top 10% earn 'Arena Champion' badge contributing bonus points 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 46 


## Page 47

 
 
 
 
Innovation Leaderboard (Creative): 
●​ Ranked by Innovation Points earned through Sandbox discoveries and novel approaches 
●​ Peer-nominated innovations receive 2x point multiplier 
●​ Monthly 'Most Innovative Design' award selected by Technical Review Panel 
●​ Innovation leaderboard position contributes to D8 scoring dimension 
 
 
 
Strictly Private and Confidential - Not for Circulation                                                                          Page number: 47 


## Page 48

 
 
 
 
